---
title: "Symfony 4 и Doctrine 2: Автоматические префиксы для таблиц"
description: В моем проекте на Symfony понадобились префиксы для таблиц, чтобы таблицы "по умолчанию" отличались от других(будущих) таблиц. Решил написать небольшой пост на эту тему.
category: blog
tag: "Symphony/Doctrine"
lang: ru
---
>В моем проекте на Symfony понадобились префиксы для таблиц, чтобы таблицы "по умолчанию" отличались от других(будущих) таблиц. Решил написать небольшой пост на эту тему.

На сайте Doctrine есть страница, описывающая реализацию слушателя для смены префикса таблиц, вот <a target="_blank" href="https://www.doctrine-project.org/projects/doctrine-orm/en/2.6/cookbook/sql-table-prefixes.html">ссылка</a>

Используем ее и в нашем проекте заведем класс-слушатель `TablePrefixSubscriber` и разместим его в `/src/EventListener/TablePrefixSubscriber.php`, получаем вот такое содержимое:

```php
declare(strict_types=1);

namespace App\EventListener;

use Doctrine\ORM\Event\LoadClassMetadataEventArgs;
use Doctrine\Common\EventSubscriber;

class TablePrefixSubscriber implements EventSubscriber
{
    /**
     * Tables prefix
     * @var string
     */
    protected $prefix = '';

    /**
     * TablePrefixSubscriber constructor.
     * @param $prefix
     */
    public function __construct($prefix)
    {
        $this->prefix = (string) $prefix;
    }

    /**
     * @return array
     */
    public function getSubscribedEvents() : array
    {
        return array('loadClassMetadata');
    }

    /**
     * @param LoadClassMetadataEventArgs $args
     */
    public function loadClassMetadata(LoadClassMetadataEventArgs $args)
    {
        $classMetadata = $args->getClassMetadata();

        if (!$classMetadata->isInheritanceTypeSingleTable() || $classMetadata->getName() === $classMetadata->rootEntityName) {
            $classMetadata->setPrimaryTable([
                'name' => $this->prefix . $classMetadata->getTableName()
            ]);
        }

        foreach ($classMetadata->getAssociationMappings() as $fieldName => $mapping) {
            if ($mapping['type'] == \Doctrine\ORM\Mapping\ClassMetadataInfo::MANY_TO_MANY && $mapping['isOwningSide']) {
                $mappedTableName = $mapping['joinTable']['name'];
                $classMetadata->associationMappings[$fieldName]['joinTable']['name'] = $this->prefix . $mappedTableName;
            }
        }
    }
}
```

В конфигах сервисов проекта в файле `services.yaml` добавляем новый параметр `app.table_prefix: <сам префикс>`, вот так выглядит часть кода:
```yaml
parameters:
    locale: 'ru'
    # This parameter defines the codes of the locales (languages) enabled in the application
    app_locales: en|fr|de|es|cs|nl|ru|uk|ro|pt_BR|pl|it|ja|id|ca|sl|hr|zh_CN|bg|tr|lt
    app.notifications.email_sender: anonymous@anonymous
    app.table_prefix: prefix_
```

а так же регистрируем сервис в этом же файле на базе подготовленного класса и тегируем его `doctrine.event_subscriber`, и еще передаем дефолтный параметр в аргументы `app.table_prefix`:
```yaml
App\EventListener\TablePrefixSubscriber:
        arguments: ['%app.table_prefix%']
        tags:
            - { name: doctrine.event_subscriber }
```
В итоге при генерации миграций и т.п., к примеру
```bash 
php bin/console doctrine:migrations:diff
```

названия таблиц будут генерироваться с префиксом, указанным в **app.table_prefix**.
