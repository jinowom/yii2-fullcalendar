# yii2-fullcalendar
Yii2全日历小部件,Fullcalendar Yii2扩展JQuery来自：http ://arshaw.com/fullcalendar/ 版本4.0.2许可证MIT

Installation
============
Package is although registered at packagist.org - so you can just add one line of code, to let it run!

add the following line to your composer.json require section:
```json
  "jinowom/yii2-fullcalendar":"*",
```

or run:
```
$ php composer.phar require jinowom/yii2fullcalendar "*"
```

And ensure, that you have the following plugin installed global:

> php composer.phar global require "fxp/composer-asset-plugin:~1.0"

Changelog
---------

17-04-2019 Updated to latest 4.0.2 Stable Version of the library
19-01-2017 Updated to include non-standard fields
29-11-2014 Updated to latest 2.2.3 Version of the library

Usage
=====

Quickstart Looks like this:

```php

  $events = array();
  //Testing
  $Event = new \jinowom\yii2fullcalendar\models\Event();
  $Event->id = 1;
  $Event->title = 'Testing';
  $Event->start = date('Y-m-d\TH:i:s\Z');
  $Event->nonstandard = [
    'field1' => 'Something I want to be included in object #1',
    'field2' => 'Something I want to be included in object #2',
  ];
  $events[] = $Event;

  $Event = new jinowom\yii2fullcalendar\models\Event();
  $Event->id = 2;
  $Event->title = 'Testing';
  $Event->start = date('Y-m-d\TH:i:s\Z',strtotime('tomorrow 6am'));
  $events[] = $Event;

  ?>

  <?= jinowom\yii2fullcalendar\yii2fullcalendar::widget(array(
      'events'=> $events,
  ));
```

Note, that this will only view the events without any detailed view or option to add a new event.. etc.

Non-Standard fields
===================

You can add non-standard fields via the non-standard fields array, for which you can pass any key/value pair, as described in the [Event Fields](https://fullcalendar.io/docs/event_data/Event_Object/) documentation.

So, using the Quick Start example above, you can read `field1` and `fields2` in your JavaScript using notation similar to `event.nonstandard.field1` and `event.nonstandard.field2`.

AJAX Usage
==========
If you wanna use ajax loader, this could look like this:

# 20171023 ajaxEvents are replaced by events - pls. check fullcalendar io documentation for details

```php
<?= jinowom\yii2fullcalendar\yii2fullcalendar::widget([
      'options' => [
        'lang' => 'de',
        //... more options to be defined here!
      ],
      'events' => Url::to(['/timetrack/default/jsoncalendar'])
    ]);
?>
```

and inside your referenced controller, the action should look like this:

```php
public function actionJsoncalendar($start=NULL,$end=NULL,$_=NULL){

    \Yii::$app->response->format = \yii\web\Response::FORMAT_JSON;

    $times = \app\modules\timetrack\models\Timetable::find()->where(array('category'=>\app\modules\timetrack\models\Timetable::CAT_TIMETRACK))->all();

    $events = array();

    foreach ($times AS $time){
      //Testing
      $Event = new \jinowom\yii2fullcalendar\models\Event();
      $Event->id = $time->id;
      $Event->title = $time->categoryAsString;
      $Event->start = date('Y-m-d\TH:i:s\Z',strtotime($time->date_start.' '.$time->time_start));
      $Event->end = date('Y-m-d\TH:i:s\Z',strtotime($time->date_end.' '.$time->time_end));
      $events[] = $Event;
    }

    return $events;
  }
```
