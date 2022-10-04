# Usage
This package provides a set of classes which are message sources that store translated messages in files.
They extends from [`yii\i18n\PhpMessageSource`](https://www.yiiframework.com/doc/api/2.0/yii-i18n-phpmessagesource) class, so their usage is basically the same.

!!! tip
    See the [Yii guide](https://www.yiiframework.com/doc/guide/2.0/en/tutorial-i18n#message-translation) for more information about internationalization and message translation.

### Using a JSON catalog
In your application configuration file, use the following message source:

```php
<?php
return [
  'components' => [
    'i18n' => [
      'translations' => [
        'app*' => 'yii\i18n\JsonMessageSource'
      ]
    ]
  ]
];
```

Within a [JSON](https://json.org) file, an object literal of (source, translation) pairs provides the message catalog. For example:

```json
{
  "original message 1": "translated message 1",
  "original message 2": "translated message 2"
}
```

### Using a YAML catalog
In your application configuration file, use the following message source:

```php
<?php
return [
  'components' => [
    'i18n' => [
      'translations' => [
        'app*' => 'yii\i18n\YamlMessageSource'
      ]
    ]
  ]
];
```

Within a [YAML](http://yaml.org) file, an object literal of (source, translation) pairs provides the message catalog. For example:

```yaml
"original message 1": "translated message 1"
"original message 2": "translated message 2"
```

## Nested objects
It is a common practice to use keys instead of original messages, alongside the enablement of the [`yii\i18n\MessageSource::$forceTranslation`](https://www.yiiframework.com/doc/api/2.0/yii-i18n-messagesource#$forceTranslation-detail) property. For example:

```json
{
  "foo.bar": "translated message for key 'foo.bar'",
  "bar.qux": "translated message for key 'bar.qux'"
}
```

The `yii\i18n\JsonMessageSource` and `yii\i18n\YamlMessageSource` classes understand nested objects.
This means that you can have JSON files that look like this:

```json
{
  "foo": "translated message for key path 'foo'",
  "bar": {
    "qux": "translated message for key path 'bar.qux'"
  },
  "baz": {
    "qux": {
      "quux": "translated message for key path 'baz.qux.quux'"
    }
  }
}
```

Or YAML files that look like that:

```yaml
foo: "translated message for key path 'foo'"
bar:
  qux: "translated message for key path 'bar.qux'"
baz:
  qux:
    quux: "translated message for key path 'baz.qux.quux'"
```

And use dot notation to access a translation:

```php
<?php
function main(): void {
  echo \Yii::t('app', 'baz.qux.quux');
  // Prints: translated message for key path 'baz.qux.quux'
}
```

To optin for this feature, you must set the `$enableNesting` property.
In the application configuration file:

```php
<?php
return [
  'components' => [
    'i18n' => [
      'translations' => [
        'app*' => [
          'class' => 'yii\i18n\JsonMessageSource',
          'enableNesting' => true
        ]
      ]
    ]
  ]
];
```

If you don't want to use the dot notation, you can customize the character(s) used by setting the `$nestingSeparator` property. For example, to use the slash character (e.g. `/`) as property delimiter:

```php
<?php
return [
  'components' => [
    'i18n' => [
      'translations' => [
        'app*' => [
          'class' => 'yii\i18n\YamlMessageSource',
          'enableNesting' => true,
          'nestingSeparator' => '/'
        ]
      ]
    ]
  ]
];
```

You can then access a translation by using the customized notation:

```php
<?php
function main(): void {
  // Using the slash character.
  echo \Yii::t('app', 'foo/bar/baz');

  // Using the "->" string.
  echo \Yii::t('app', 'foo->bar->baz');
}
```

!!! tip
    This package also provides the `yii\i18n\ExtendedPhpMessageSource` class.
    This class adds nested objects support to the [`yii\i18n\PhpMessageSource`](https://www.yiiframework.com/doc/api/2.0/yii-i18n-phpmessagesource) class.
