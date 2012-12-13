#li3_model

#### A more flexible model that updated version of the [Lithium](https://github.com/UnionOfRAD/lithium) framework

## Model Creation and Configurtion

Lithium provides you with a general-purpose class that all your models should extend. You can find the Model class in the `lithium\data` namespace. If you do nothing more than extend it, you instantly get a bunch of functionality that covers basic CRUD as well as more complex tasks.

Let's say you want to store and manage blog posts in your database. According to Lithium conventions, you create a new file called Posts.php in app/models. The basic structure looks like this:

```php
<?php

namespace app\models;

class Posts extends \lithium\data\Model {
}

?>
```

If you don't specify otherwise, your model will use the `default` connection specified in `app/config/connections.php` (along with some other default configuration settings). All these defaults are stored in the model's `$_meta` variable and can be overridden if the defaults don't fit your needs. Let's say you want to use the `backup` connection instead of the default one.

```php
<?php

namespace app\models;

class Posts extends \lithium\data\Model {

	protected static $_meta = array(
			'connection' => 'backup',
			'source' => 'tableName',
			'key' => 'id'
	);
?>
```

### Note:
In the `$_meta` `key` should be mapped to a primary key that exits in your tableName which specified in the `source`.

## Basic CURD

#### introduce several new features which make the freamwork more flexible and awsome.now let's start.

### Retrieving Data

### Saving Data

Persisting data means that new data is being stored or updated. Before you can save your data, you have to initialize a `Model`. This can either be done with the `find()` methods or—if you want to create a new record—with the `create()` method. You then have various ways to add or modify your data. Here are some examples:

```php
<?php
// Find the post record
$post 		  = $this->find('first');
$post->title  = 'my first post';
$post->author = 'asturn';
$post->save('update');


// Create a new post,add title and author,then save
$data = array(
		'title' => 'the first blog post';
		'author' => 'asturn';
		);
$success = $this->create($data);
if ($success) {
	$id = $success->save();
} else {
	return $this->errors('some words about errors');
}
?>
```
If the creation succeed,depending on the result of the validation,it will return the last insert id.Maybe you can use the `insert()` function directly like so:

```php
<?php
// Create a new post,add title and author,then save
$data = array(
		'title' => 'the second blog post';
		'author' => 'asturn';
		);
//return boolen `true` or `false`
$success = $this->insert($data);
?>
```

### Updating Data

The last section showed you how to save documents/records in your database. If these records existed previously, you are also updating them. Utilise `update()` method which updates records directly in your database (similar to the sql `UPDATE` command).
The first argument provides the data to change, the second (optional) one is a set of conditions to narrow down your selection. Here are some examples:

```php
<?php
public function updateTitle($id, $newTitle) {
	return $this->update(
			array('title' => $date),
			array('id' => $id)
			);
}
?>

### Deleting Data
Removing data works similar to updating data, now use the `delete()` function you can remove a subset of your database entries.

```php
<?php
// Delete all posts with an empty title
$result = $this->delete(array('title' => ''));
?>
```

Be careful with this.If you don't provide any arguments to `delete()`, then all documents/rows in your database will be deleted!