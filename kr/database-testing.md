# Database Testing
# 데이터베이스 테스팅

- [Introduction](#introduction)
- [소개하기](#introduction)
- [Resetting The Database After Each Test](#resetting-the-database-after-each-test)
- [각각의 테스트 수행 후에 데이터베이스 재설정하기](#resetting-the-database-after-each-test)
    - [Using Migrations](#using-migrations)
    - [마이그레이션 이용하기](#using-migrations)
    - [Using Transactions](#using-transactions)
    - [트랜잭션 이용하기](#using-transactions)
- [Writing Factories](#writing-factories)
- [팩토리 작성하기](#writing-factories)
    - [Factory Types](#factory-types)
    - [팩토리 타입들](#factory-types)
- [Using Factories](#using-factories)
- [팩토리 사용하기](#using-factories)
    - [Creating Models](#creating-models)
    - [모델 생성하기](#creating-models)
    - [Persisting Models](#persisting-models)
    - [모델 저장하기](#persisting-models)
    - [Relationships](#relationships)
    - [관계-Relationships](#relationships)

<a name="introduction"></a>
## Introduction
## 소개하기

Laravel provides a variety of helpful tools to make it easier to test your database driven applications. First, you may use the `seeInDatabase` helper to assert that data exists in the database matching a given set of criteria. For example, if you would like to verify that there is a record in the `users` table with the `email` value of `sally@example.com`, you can do the following:

라라벨은 데이터베이스를 기반으로 하는 어플리케이션을 손쉽게 테스트 할수 있도록 도와주는 다양한 툴을 제공합니다. 우선 `seeInDatabase` 헬퍼를 이용하여 데이터베이스 안에 특정 조건을 만족하는 데이터가 존재하는지 확인할 수 있습니다. 예를 들어, `users` 테이블에 `sally@example.com`의 `email` 값을 가진 레코드가 존재하는지 확인하기 위해 다음과 같이 할 수 있습니다:

    public function testDatabase()
    {
        // Make call to application...

        $this->seeInDatabase('users', [
            'email' => 'sally@example.com'
        ]);
    }

Of course, the `seeInDatabase` method and other helpers like it are for convenience. You are free to use any of PHPUnit's built-in assertion methods to supplement your tests.

<a name="resetting-the-database-after-each-test"></a>
## Resetting The Database After Each Test
## 각각의 테스트 수행 후에 데이터베이스 재설정하기 

It is often useful to reset your database after each test so that data from a previous test does not interfere with subsequent tests.

종종 이전의 테스트를 위한 데이터가 다음에 이어지는 테스트들을 방해하는 것을 방지하기 위해 각 테스트 후에 데이터베이스를 재설정하는 것이 유용합니다. 

<a name="using-migrations"></a>
### Using Migrations
### 마이그레이션 이용하기

One approach to resetting the database state is to rollback the database after each test and migrate it before the next test. Laravel provides a simple `DatabaseMigrations` trait that will automatically handle this for you. Simply use the trait on your test class and everything will be handled for you:

데이터베이스를 재설정하는 하나의 방법은 각각의 테스트 수행 후에 데이터베이스를 롤백하고 다음 테스트를 수행하기 전에 다시 마이그레이션을 실행하는 것입니다. 라라벨은 `DatabaseMigrations` 트레이트-trait을 제공하여 이를 자동으로 처리 해줍니다. 테스트 클래스에서 이 트레이트-trait을 사용하기만 하면, 모든게 처리될것입니다:

    <?php

    use Illuminate\Foundation\Testing\WithoutMiddleware;
    use Illuminate\Foundation\Testing\DatabaseMigrations;
    use Illuminate\Foundation\Testing\DatabaseTransactions;

    class ExampleTest extends TestCase
    {
        use DatabaseMigrations;

        /**
         * A basic functional test example.
         *
         * @return void
         */
        public function testBasicExample()
        {
            $this->visit('/')
                 ->see('Laravel 5');
        }
    }

<a name="using-transactions"></a>
### Using Transactions
### 트랜잭션 이용하기 

Another approach to resetting the database state is to wrap each test case in a database transaction. Again, Laravel provides a convenient `DatabaseTransactions` trait that will automatically handle this for you:

데이터베이스를 재설정하는 다른 방법으로는 데이터베이스 트랜잭션으로 모든 테스트 케이스를 감싸는 것입니다. 이 때에도 라라벨은 편리하게 `DatabaseTransactions` 트레이트-trait을 제공하여 이것이 자동으로 처리되도록 합니다: 

    <?php

    use Illuminate\Foundation\Testing\WithoutMiddleware;
    use Illuminate\Foundation\Testing\DatabaseMigrations;
    use Illuminate\Foundation\Testing\DatabaseTransactions;

    class ExampleTest extends TestCase
    {
        use DatabaseTransactions;

        /**
         * A basic functional test example.
         *
         * @return void
         */
        public function testBasicExample()
        {
            $this->visit('/')
                 ->see('Laravel 5');
        }
    }

> {note} This trait will only wrap the default database connection in a transaction. If your application is using multiple database connections, you will need to manually handle the transaction logic for those connections.

> {note} 이 트레이트-trait은 트랜잭션의 기본 데이터베이스 커넥션만을 둘러싸게 됩니다. 만약 어플리케이션에서 여러개의 데이터베이스 커넥션을 사용중이라면, 수동으로 이 커넥션들을 위한 트랙잭션 처리를 할 필요가 있을 것입니다.

<a name="model-factories"></a>
## Writing Factories
## 팩토리 작성하기

When testing, it is common to need to insert a few records into your database before executing your test. Instead of manually specifying the value of each column when you create this test data, Laravel allows you to define a default set of attributes for each of your [Eloquent models](/docs/{{version}}/eloquent) using model factories. To get started, take a look at the `database/factories/ModelFactory.php` file in your application. Out of the box, this file contains one factory definition:

테스트를 실행하기 전에 흔히 데이터베이스에 몇몇 레코드를 입력하는 것이 필요할 때가 있습니다. 이 테스트 데이터를 생성할 때 수동으로 각각의 컬럼의 값을 지정하는 대신에 라라벨은 모델 팩토리를 사용하여 각각의 [Eloquent 모델](/docs/{{version}}/eloquent)을 위한 기본 속성의 세트를 정의하도록 해줍니다. 먼저 어플리케이션의 `database/factories/ModelFactory.php` 파일을 살펴보겠습니다. 이 파일은 바로 사용이 가능한 하나의 팩토리 정의를 가지고 있습니다. 

    $factory->define(App\User::class, function (Faker\Generator $faker) {
        return [
            'name' => $faker->name,
            'email' => $faker->email,
            'password' => bcrypt(str_random(10)),
            'remember_token' => str_random(10),
        ];
    });

Within the Closure, which serves as the factory definition, you may return the default test values of all attributes on the model. The Closure will receive an instance of the [Faker](https://github.com/fzaninotto/Faker) PHP library, which allows you to conveniently generate various kinds of random data for testing.

팩토리의 정의를 제공하는 클로저 안에서 모든 모델의 속성의 기본 테스트 값을 반환할 수 있습니다. 클로저는 [Faker](https://github.com/fzaninotto/Faker) PHP 라이브러리 인스턴스를 전달 받을 것이고, 이는 테스트할 수 있는 다양한 랜덤 데이터를 편리하게 생성할 수 있게 해줍니다. 

Of course, you are free to add your own additional factories to the `ModelFactory.php` file. You may also create additional factory files for each model for better organization. For example, you could create `UserFactory.php` and `CommentFactory.php` files within your `database/factories` directory. All of the files within the `factories` directory will automatically be loaded by Laravel.

물론 `ModelFactory.php` 파일에 여러분의 고유한 팩토리들을 자유롭게 추가할 수 있습니다. 여러분은 또한 보다 나은 구성을 위해서 각각의 모델들을 위한 추가적인 팩토리 파일들을 생성할 수도 있습니다. 예를 들어 `database/factories` 디렉토리에 `UserFactory.php` 와 `CommentFactory.php` 파일을 생성할 수 있습니다. `factories` 디렉토리 안에 들어 있는 모든 파일들은 라라벨에 의해서 자동으로 로딩됩니다. 

<a name="factory-types"></a>
### Factory Types
### 팩토리 타입들

Sometimes you may wish to have multiple factories for the same Eloquent model class. For example, perhaps you would like to have a factory for "Administrator" users in addition to normal users. You may define these factories using the `defineAs` method:

여러분은 종종 동일한 Eloquent 모델 클래스를 위해 여러 개의 팩토리들을 가지고자 할수도 있습니다. 예를 들어, 보통 사용자들 외에도 "관리자" 사용자들을 위한 팩토리를 원할 수 있습니다. `defineAs` 메소드로 이 팩토리들을 정의 할 수 있습니다.

    $factory->defineAs(App\User::class, 'admin', function ($faker) {
        return [
            'name' => $faker->name,
            'email' => $faker->email,
            'password' => str_random(10),
            'remember_token' => str_random(10),
            'admin' => true,
        ];
    });

Instead of duplicating all of the attributes from your base user factory, you may use the `raw` method to retrieve the base attributes. Once you have the attributes, simply supplement them with any additional values you require:

베이스 사용자 팩토리에서 모든 속성들을 복제하는 대신에, 기본 속성들을 조회하기 위해서 `raw` 메소드를 사용할 수 있습니다. 속성들을 조회한 뒤에는 필요한 어떤 값들로도 보완 할 수 있습니다. 

    $factory->defineAs(App\User::class, 'admin', function ($faker) use ($factory) {
        $user = $factory->raw(App\User::class);

        return array_merge($user, ['admin' => true]);
    });

<a name="using-factories"></a>
## Using Factories
## 팩토리 사용하기

<a name="creating-models"></a>
### Creating Models
### 모델 생성하기

Once you have defined your factories, you may use the global `factory` function in your tests or seed files to generate model instances. So, let's take a look at a few examples of creating models. First, we'll use the `make` method to create models but not save them to the database:

팩토리를 정의한 뒤에는, 테스트파일 에서나 모델 인스턴스를 생성하기 위한 시드 파일안에서 글로벌 `factory` 함수를 사용할 수 있습니다. 그럼 모델들을 생성하는 몇몇의 예제를 살펴보겠습니다. 우선 모델을 생성하지만 데이터베이스에 저장하지는 않는, `make` 메소드를 사용할 것입니다: 

    public function testDatabase()
    {
        $user = factory(App\User::class)->make();

        // Use model in tests...
    }

You may also create a Collection of many models or create models of a given type:

또한 여러 모델의 컬렉션을 생성하거나 주어진 타입의 모델을 생성할 수도 있습니다: 

    // Create three App\User instances...
    $users = factory(App\User::class, 3)->make();

    // Create an "admin" App\User instance...
    $user = factory(App\User::class, 'admin')->make();

    // Create three "admin" App\User instances...
    $users = factory(App\User::class, 'admin', 3)->make();

#### Overriding Attributes
#### 속성 재지정(오버라이딩) 하기

If you would like to override some of the default values of your models, you may pass an array of values to the `make` method. Only the specified values will be replaced while the rest of the values remain set to their default values as specified by the factory:

모델의 몇몇 기본값들을 대체하고 싶다면 `make` 메소드로 값들의 배열을 전달하면 됩니다. 지정된 값들만 대체되고 나머지 값들은 팩토리에 의해 지정된 기본값들을 유지할 것입니다. 

    $user = factory(App\User::class)->make([
        'name' => 'Abigail',
    ]);

<a name="persisting-models"></a>
### Persisting Models
### 모델 저장히기

The `create` method not only creates the model instances but also saves them to the database using Eloquent's `save` method:

`create` 메소드는 모델 인스턴스를 단지 생성하기만 하기 때문에 데이터베이스에 저장하려면, Eloquent의 `save` 메소드를 사용하면 됩니다: 

    public function testDatabase()
    {
        // Create a single App\User instance...
        $user = factory(App\User::class)->create();

        // Create three App\User instances...
        $users = factory(App\User::class, 3)->create();

        // Use model in tests...
    }

You may override attributes on the model by passing an array to the `create` method:

`create` 메소드에 배열을 전달하여 모델의 속성들을 재지정 할 수 있습니다: 

    $user = factory(App\User::class)->create([
        'name' => 'Abigail',
    ]);

<a name="relationships"></a>
### Relationships
### 관계(relationships)

In this example, we'll attach a relation to some created models. When using the `create` method to create multiple models, an Eloquent [collection instance](/docs/{{version}}/eloquent-collections) is returned, allowing you to use any of the convenient functions provided by the collection, such as `each`:

이 예제에서는, 생성된 몇몇의 모델에 관계를 추가할 것입니다. 여러 모델을 생성하기 위해서 `create` 메소드를 사용할 때 Eloquent [컬렉션 인스턴스](/docs/{{version}}/eloquent-collections)가 반환되어 `each`와 같이 컬렉션이 제공하는 편리한 함수들을 이용할 수 있도록 해줍니다: 

    $users = factory(App\User::class, 3)
               ->create()
               ->each(function ($u) {
                    $u->posts()->save(factory(App\Post::class)->make());
                });

#### Relations & Attribute Closures
#### 관계 & 속성 클로저-Closures

You may also attach relationships to models using Closure attributes in your factory definitions. For example, if you would like to create a new `User` instance when creating a `Post`, you may do the following:

팩토리 정의안에서 클로저를 속성으로 사용하여 모델에 관계를 추가할 수도 있습니다. 예를 들어 `Post` 가 생성될 때 `User` 인스턴스를 생성하고자 한다면, 다음처럼 하면 됩니다:

    $factory->define(App\Post::class, function ($faker) {
        return [
            'title' => $faker->title,
            'content' => $faker->paragraph,
            'user_id' => function () {
                return factory(App\User::class)->create()->id;
            }
        ];
    });

These Closures also receive the evaluated attribute array of the factory that contains them:

이 클로저는 팩토리의 계산된 속성 배열을 받을 수도 있습니다. 

    $factory->define(App\Post::class, function ($faker) {
        return [
            'title' => $faker->title,
            'content' => $faker->paragraph,
            'user_id' => function () {
                return factory(App\User::class)->create()->id;
            },
            'user_type' => function (array $post) {
                return App\User::find($post['user_id'])->type;
            }
        ];
    });