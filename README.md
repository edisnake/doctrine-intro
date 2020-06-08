Introduction to Doctrine
========================

### Prerequisites

PHP and Composer are ready to use on your local environment. For the purposes of this exercise PHP is installed locally 
and Composer is installed globally. Visit [PHP installation](https://www.php.net/manual/en/install.php) and 
[Composer installation](https://getcomposer.org/doc/00-intro.md#installation-linux-unix-macos) for more. 

#### Initial commit

For the initial commit we only run the commands to have a  Symfony skeleton application and declare our dependency of 
Doctrine. For this introduction we are using the [Doctrine ORM Pack](https://packagist.org/packages/symfony/orm-pack). 

```bash
# create project
composer create-project symfony/skeleton doctrine-intro

# add doctrine dependency
composer require symfony/orm-pack
composer require --dev symfony/maker-bundle
```

Additionally we are using the `maker-bundle` from Symfony in order to use Doctrine's helper commands to generate 
files/code. More on [Symfony packs](https://symfony.com/doc/current/setup.html#symfony-packs).

#### Add entities with primitive properties

In this commit we will first set up our Doctrine settings. For the purposes of this introduction we will use SQLite. All
we have to do is change the `DATABASE_URL` parameter in our `.env` to `sqlite:///%kernel.project_dir%/var/data.db`.

In order to test that you have a successful database connection run the following command: 

```bash
bin/console doctrine:database:create
```

If everything is successful you should read `Created database .../doctrine-intro/var/data.db for connection named default`.
Where `...` is your local directory location.

Now we can continue to create our entities. We will work with [Publisher](doc/create/make-publisher.md), [Book](doc/create/make-book.md), 
[Author](doc/create/make-author.md) and [Address](doc/create/make-address.md). All of them will first 
have primitive property values. To create them we run the following command for each entity.

```bash
bin/console make:entity
```

Last we run the following command to synchronise our physical database schema with our application's schema.

```bash
bin/console doctrine:schema:create
```

You should see a warning telling you that this operation should not be executed in a production environment and a success 
message stating `[OK] Database schema created successfully!`

#### Add create, find and remove commands for Publisher, Book, Author and Address

In this commit we are creating simple commands to showcase the main usage of persisting (`INSERT`), finding (`SELECT`) 
and removing (`DELETE`) entities from our storage/database. 

#### Add relationships between entities

In this commit we make use again of the `make:entity` command multiple times to define relationships between our 
[Publisher](doc/update/make-publisher.md), [Book](doc/update/make-book.md), [Author](doc/update/make-author.md) and 
Address entities. The relationship structure is shown in the table below: 

| Which?    | Target    | Relationship             |
|-----------|-----------|--------------------------|
| Publisher | Address   | OneToOne unidirectional  |
| Book      | Publisher | ManyToOne unidirectional |
| Book      | Author    | ManyToMany bidirectional |
| Author    | Address   | OneToMany bidirectional  |

#### Update create, find commands for Publisher, Book, Author for relationships

In this commit we update the create and find commands to include the new relationship definitions. We also change slightly
doctrine's default generated code.  

* We make Publisher - Address a read only interaction.
* We change Book relationship fetching mechanism to better suit our needs.
