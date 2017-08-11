# Blade Templates
# 블레이드 템플릿

- [Introduction](#introduction)
- [소개하기](#introduction)
- [Template Inheritance](#template-inheritance)
- [템플릿 상속](#template-inheritance)
    - [Defining A Layout](#defining-a-layout)
    - [레이아웃 정의하기](#defining-a-layout)
    - [Extending A Layout](#extending-a-layout)
    - [레이아웃 확장하기](#extending-a-layout)
- [Components & Slots](#components-and-slots)
- [컴포넌트 & 슬롯](#components-and-slots)
- [Displaying Data](#displaying-data)
- [데이터 표시하기](#displaying-data)
    - [Blade & JavaScript Frameworks](#blade-and-javascript-frameworks)
    - [블레이드 & 자바스크립트 프레임워크](#blade-and-javascript-frameworks)
- [Control Structures](#control-structures)
- [제어 구조](#control-structures)               
    - [If Statements](#if-statements)
    - [조건문](#if-statements)
    - [Loops](#loops)
    - [반복문](#loops)
    - [The Loop Variable](#the-loop-variable)
    - [루프 변수](#the-loop-variable)
    - [Comments](#comments)
    - [주석](#comments)
    - [PHP](#php)
    - [PHP](#php)
- [Including Sub-Views](#including-sub-views)
- [하위 뷰파일 포함시키기](#including-sub-views)
    - [Rendering Views For Collections](#rendering-views-for-collections)
    - [컬렉션을 뷰에서 렌더링하기](#rendering-views-for-collections)
- [Stacks](#stacks)
- [스택](#stacks)
- [Service Injection](#service-injection)
- [서비스 인젝션-주입](#service-injection)
- [Extending Blade](#extending-blade)
- [블레이드 기능 확장하기](#extending-blade)

<a name="introduction"></a>
## Introduction
## 소개하기

Blade is the simple, yet powerful templating engine provided with Laravel. Unlike other popular PHP templating engines, Blade does not restrict you from using plain PHP code in your views. In fact, all Blade views are compiled into plain PHP code and cached until they are modified, meaning Blade adds essentially zero overhead to your application. Blade view files use the `.blade.php` file extension and are typically stored in the `resources/views` directory.

블레이드는 라라벨에서 제공하는 간단하지만 강력한 템플릿 엔진입니다. 다른 인지도 높은 PHP 템플릿 엔진들과는 달리 블레이드는 뷰에서 순수한 PHP 코드를 작성하는 것을 허용합니다. 실제로는, 모든 블레이드 뷰는 단순한 PHP 코드로 컴파일되고 변경되기 전까지 캐시 됩니다. 이는 블레이드가 어플리케이션에 아무런 부담을 주지 않는다는 것을 의미합니다. 블레이드 뷰 파일은 `.blade.php` 형식의 파일 확장를 사용하고 주로 `resources/views`에 저장됩니다. 

<a name="template-inheritance"></a>
## Template Inheritance
## 템플릿 상속

<a name="defining-a-layout"></a>
### Defining A Layout
### 레이아웃 정의하기

Two of the primary benefits of using Blade are _template inheritance_ and _sections_. To get started, let's take a look at a simple example. First, we will examine a "master" page layout. Since most web applications maintain the same general layout across various pages, it's convenient to define this layout as a single Blade view:

블레이드의 가장 주요한 두가지 장점은 _템플릿 상속_ 과 _섹션_ 입니다. 먼저 간단한 예를 살펴보겠습니다. 우선 "마스터" 페이지 레이아웃을 구성할 것입니다. 대부분의 웹 어플리케이션이 다양한 페이지에서 동일한 레이아웃을 유지하기 때문에 이 레이아웃을 하나의 블레이드 뷰로 정의하는 것이 편리합니다:

    <!-- Stored in resources/views/layouts/app.blade.php -->

    <html>
        <head>
            <title>App Name - @yield('title')</title>
        </head>
        <body>
            @section('sidebar')
                This is the master sidebar.
            @show

            <div class="container">
                @yield('content')
            </div>
        </body>
    </html>

As you can see, this file contains typical HTML mark-up. However, take note of the `@section` and `@yield` directives. The `@section` directive, as the name implies, defines a section of content, while the `@yield` directive is used to display the contents of a given section.

여기서 볼 수 있듯이, 이 파일은 전형적인 HTML 마크업을 가지고 있습니다. 하지만 `@section` 와 `@yield` 지시어을 확인하십시오. `@section` 는 말 그대로 내용의 섹션을 정의하고 `@yield`는 어떤 섹션의 컨텐츠을 나타내는 데에 사용됩니다.

Now that we have defined a layout for our application, let's define a child page that inherits the layout.

이제 어플리케이션의 레이아웃을 정의하였으니, 이 레이아웃을 상속하는 자식 페이지를 정의하도록 하겠습니다. 

<a name="extending-a-layout"></a>
### Extending A Layout
### 레이아웃 확장하기

When defining a child view, use the Blade `@extends` directive to specify which layout the child view should "inherit". Views which extend a Blade layout may inject content into the layout's sections using `@section` directives. Remember, as seen in the example above, the contents of these sections will be displayed in the layout using `@yield`:

하위 뷰를 정의할 때 블레이드 `@extends` 지시어을 사용해 하위 페이지가 어느 레이아웃을 "상속" 받을지 명시할 수 있습니다. 블레이드 레이아웃을 상속 받는 뷰는 `@section` 지시어를 이용해 레이아웃의 섹션에 컨텐츠를 삽입할 수 있습니다. 위의 예제에서 본 것처럼 이 섹션들의 컨텐츠는 `@yield`를 통해 레이아웃에 명시됩니다: 

    <!-- Stored in resources/views/child.blade.php -->

    @extends('layouts.app')

    @section('title', 'Page Title')

    @section('sidebar')
        @@parent

        <p>This is appended to the master sidebar.</p>
    @endsection

    @section('content')
        <p>This is my body content.</p>
    @endsection

In this example, the `sidebar` section is utilizing the `@@parent` directive to append (rather than overwriting) content to the layout's sidebar. The `@@parent` directive will be replaced by the content of the layout when the view is rendered.

이 예제에 `sidebar` 섹션은 `@@parent` 지시어를 활용해서 레이아웃 사이드바에 컨텐츠를 겹쳐 쓰지 않고 추가합니다. `@@parent` 지시어은 뷰가 렌더링되면 레이아웃의 컨텐츠에 의해 대체됩니다. 

Blade views may be returned from routes using the global `view` helper:

블레이드 뷰도 글로벌 `view` 헬퍼를 사용하여 라우트에서 반환될 수 있습니다: 

    Route::get('blade', function () {
        return view('child');
    });

<a name="components-and-slots"></a>
## Components & Slots
## 컴포넌트 & 슬롯
 
Components and slots provide similar benefits to sections and layouts; however, some may find the mental model of components and slots easier to understand. First, let's imagine a reusable "alert" component we would like to reuse throughout our application:
 
컴포넌트와 슬롯은 섹션 및 레이아웃과 유사한 장점을 제공합니다. 컴포넌트와 슬롯은 결과 모델을 보다 쉽게 이해할 수 있게 해줍니다. 먼저 어플리케이션에서 재사용이 가능한 "경고(alert)" 컴포넌트를 생각해 보겠습니다. 
 
    <!-- /resources/views/alert.blade.php -->
 
    <div class="alert alert-danger">
        {{ $slot }}
    </div>
 
The `{{ $slot }}` variable will contain the content we wish to inject into the component. Now, to construct this component, we can use the `@component` Blade directive:
 
이 `{{ $slot }}` 변수는 컴포넌트에 주입될 내용을 가지고 있습니다. 이 컴포넌트를 구성하기 위해서 `@component` 블레이드 지시어를 사용할 수 있습니다:
 
    @component('alert')
        <strong>Whoops!</strong> Something went wrong!
    @endcomponent
 
Sometimes it is helpful to define multiple slots for a component. Let's modify our alert component to allow for the injection of a "title". Named slots may be displayed by simply "echoing" the variable that matches their name:
 
때로는 컴포넌트에 여러개의 슬롯을 정의하는 것이 유용합니다. "제목(title)" 주입이 가능하도록 경고(alert) 컴포넌트를 수정해보겠습니다. 이름이 지정된 슬롯은 일치하는 이름의 변수가 "출력" 되도록 표시할 수 있습니다:
 
    <!-- /resources/views/alert.blade.php -->
 
    <div class="alert alert-danger">
        <div class="alert-title">{{ $title }}</div>
 
        {{ $slot }}
    </div>
 
Now, we can inject content into the named slot using the `@slot` directive. Any content not within a `@slot` directive will be passed to the component in the `$slot` variable:

그러면 이제, `@slot` 지시어를 사용하여 이름이 지정된 슬롯에 내용을 주입할 수 있습니다. `@slot` 지시어에 포함되어 있지 않는 컨텐츠는 `$slot` 변수의 컴포넌트로 전달됩니다:
 
     @component('alert')
         @slot('title')
             Forbidden
         @endslot
 
         You are not allowed to access this resource!
     @endcomponent

#### Passing Additional Data To Components
#### 추가적인 데이터를 컴포넌트로 전달하기

Sometimes you may need to pass additional data to a component. For this reason, you can pass an array of data as the second argument to the `@component` directive. All of the data will be made available to the component template as variables:

컴포넌트로 추가적인 데이터를 전달할 필요가 있는 경우, `@component` 지시어의 두번째 인자로 데이터 배열을 전달하면 됩니다. 전달된 데이터는 컴포넌트 템플릿 에서 변수로 사용 가능합니다: 

    @component('alert', ['foo' => 'bar'])
        ...
    @endcomponent

<a name="displaying-data"></a>
## Displaying Data
## 데이터 표시하기

You may display data passed to your Blade views by wrapping the variable in curly braces. For example, given the following route:

블레이드 뷰로 전달된 데이터를 표시하기 위해 중괄호로 쌓인 변수를 전달할 수 있습니다. 예로 들어 다음의 라우트를 볼 수 있습니다:

    Route::get('greeting', function () {
        return view('welcome', ['name' => 'Samantha']);
    });

You may display the contents of the `name` variable like so:

다음과 같이 컨텐츠의 `name` 변수를 표시할 수 있습니다:

    Hello, {{ $name }}.

Of course, you are not limited to displaying the contents of the variables passed to the view. You may also echo the results of any PHP function. In fact, you can put any PHP code you wish inside of a Blade echo statement:

물론 뷰에는 전달된 변수들의 컨텐츠만 표시할 수 있는 것은 아닙니다. PHP 함수의 모든 결과는 출력될 수 있습니다. 블레이드에서는 출력되는 어떠한 PHP 코드도 넣을 수 있습니다: 

    The current UNIX timestamp is {{ time() }}.

> {note} Blade `{{ }}` statements are automatically sent through PHP's `htmlspecialchars` function to prevent XSS attacks.

> {note} 블레이드 `{{ }}` 문장들은 XSS 공격을 방지하기 위해 자동으로 PHP의 'htmlspecialchars' 함수를 통과합니다.

#### Displaying Unescaped Data
#### Escape 처리되지 않은 데이터 표시하기

By default, Blade `{{ }}` statements are automatically sent through PHP's `htmlspecialchars` function to prevent XSS attacks. If you do not want your data to be escaped, you may use the following syntax:

기본적으로 블레이드 `{{ }}` 문장은 XSS 공격을 방지하기 위해 PHP의 `htmlspecialchars` 함수를 통과합니다. 데이터를 escape 처리를 하지 않으려면 다음과 같이 작성하면 됩니다:

    Hello, {!! $name !!}.

> {note} Be very careful when echoing content that is supplied by users of your application. Always use the escaped, double curly brace syntax to prevent XSS attacks when displaying user supplied data.

> {note} 어플리케이션의 사용자들로 부터 입력하여 표시되는 컨텐츠를 출력할 때는 escape-이스케이프에 대한 주의가 필요합니다. 사용자가 제공 한 데이터를 표시 할 때 XSS 공격을 방지하려면 항상 이스케이프 처리 된 이중 중괄호 문법을 사용하십시오.

<a name="blade-and-javascript-frameworks"></a>
### Blade & JavaScript Frameworks
### 블레이드 & 자바스크립트 프레임워크

Since many JavaScript frameworks also use "curly" braces to indicate a given expression should be displayed in the browser, you may use the `@` symbol to inform the Blade rendering engine an expression should remain untouched. For example:

많은 자바스크립트 프레임워크에서 또한 중괄호를 사용하여 특정 표현이 브라우저에서 표시되어야 하는다는 것을 명시하기 때문에 `@` 기호를 써서 이 중괄호 표현을 유지해야 한다는 것을 블레이드 렌더링 엔진에게 알려 줄 수 있습니다. 예를 들어: 

    <h1>Laravel</h1>

    Hello, @{{ name }}.

In this example, the `@` symbol will be removed by Blade; however, `{{ name }}` expression will remain untouched by the Blade engine, allowing it to instead be rendered by your JavaScript framework.

이 예제에서 `@` 기호는 블레이드에 의해 지워질 것입니다. 하지만 `{{ name }}` 표현은 블레이드 엔진에 의해 영향을 받지 않을 것이며 자바스크립트 프레임워크에 의해 렌더링될 수 있게 해줍니다. 

#### The `@verbatim` Directive
#### `@verbatim` 지시어

If you are displaying JavaScript variables in a large portion of your template, you may wrap the HTML in the `@verbatim` directive so that you do not have to prefix each Blade echo statement with an `@` symbol:

여러분이 템플릿의 많은 부분에서 자바스크립트 변수를 표시하는 경우라면, HTML을 `@verbatim` 지시어로 둘러쌓여 있게 할 수 있습니다. 이렇게 하면 여러분은 각각의 블레이드 출력 구문에 `@` 심볼을 매번 붙이지 않아도 됩니다:   

    @verbatim
        <div class="container">
            Hello, {{ name }}.
        </div>
    @endverbatim

<a name="control-structures"></a>
## Control Structures
## 컨트롤 구조

In addition to template inheritance and displaying data, Blade also provides convenient shortcuts for common PHP control structures, such as conditional statements and loops. These shortcuts provide a very clean, terse way of working with PHP control structures, while also remaining familiar to their PHP counterparts.

템플릿 상속과 데이터 표시 외에도 블레이드는 공통적인 PHP 컨트롤 구조를 위해서 조건문과 반복문과 같은 편리한 방법들을 제공합니다. 이 방법들은 PHP 컨트롤 구조를 이용할 수 있는 깔끔하고 간단한 방법을 제공하면서 PHP에 익숙한 구조와 비슷하도록 유지합니다. 

<a name="if-statements"></a>
### If Statements
### 조건문

You may construct `if` statements using the `@if`, `@elseif`, `@else`, and `@endif` directives. These directives function identically to their PHP counterparts:

`@if`, `@elseif`, `@else`, 그리고 `@endif` 지시어을 이용하여 조건문을 만들 수 있습니다. 이 지시어들은 대응하는 PHP 문장들과 동일하게 동작합니다. 

    @if (count($records) === 1)
        I have one record!
    @elseif (count($records) > 1)
        I have multiple records!
    @else
        I don't have any records!
    @endif

For convenience, Blade also provides an `@unless` directive:

보다 편리하게 블레이드는 `@unless` 지시어을 제공합니다: 

    @unless (Auth::check())
        You are not signed in.
    @endunless

In addition to the conditional directives already discussed, the `@isset` and `@empty` directives may be used as convenient shortcuts for their respective PHP functions:

추가적으로 `@isset` 와 `@empty` 지시어는 각각 대응되는 PHP 함수를 편리하게 사용할 수 있는 방법입니다:  

    @isset($records)
        // $records is defined and is not null...
    @endisset

    @empty($records)
        // $records is "empty"...
    @endempty

<a name="loops"></a>
### Loops
### 반복문

In addition to conditional statements, Blade provides simple directives for working with PHP's loop structures. Again, each of these directives functions identically to their PHP counterparts:

조건문 외에도 블레이드는 PHP의 반복 구조를 위한 단순한 지시어들을 제공합니다. 이 지시어들 또한 대응하는 PHP 문장들과 동일하게 동작합니다:

    @for ($i = 0; $i < 10; $i++)
        The current value is {{ $i }}
    @endfor

    @foreach ($users as $user)
        <p>This is user {{ $user->id }}</p>
    @endforeach

    @forelse ($users as $user)
        <li>{{ $user->name }}</li>
    @empty
        <p>No users</p>
    @endforelse

    @while (true)
        <p>I'm looping forever.</p>
    @endwhile

> {tip} When looping, you may use the [loop variable](#the-loop-variable) to gain valuable information about the loop, such as whether you are in the first or last iteration through the loop.

> {tip} 반복문에서 [루프 변수](#the-loop-variable)를 사용하여 반복의 처음과 마지막에 대한 정보를 얻을 수 있습니다.  

When using loops you may also end the loop or skip the current iteration:

반복문을 사용할 때에는 반복문의 중료 또는 현재 반복의 중단을 표시할 필요가 있습니다:

    @foreach ($users as $user)
        @if ($user->type == 1)
            @continue
        @endif

        <li>{{ $user->name }}</li>

        @if ($user->number == 5)
            @break
        @endif
    @endforeach

You may also include the condition with the directive declaration in one line:

또한 하나의 라인으로 표현되는 조건식을 포함할 수도 있습니다:

    @foreach ($users as $user)
        @continue($user->type == 1)

        <li>{{ $user->name }}</li>

        @break($user->number == 5)
    @endforeach

<a name="the-loop-variable"></a>
### The Loop Variable
### 루프 변수

When looping, a `$loop` variable will be available inside of your loop. This variable provides access to some useful bits of information such as the current loop index and whether this is the first or last iteration through the loop:

반복문을 사용할 때, 반복문 안에서 `$loop` 변수를 사용할 수 있습니다. 이 변수는 현재의 반복문의 인덱스와 반복문이 첫 번째 또는 마지막 인지 알 수 있는 것과 같은 유용한 정보에 엑세스할 수 있습니다: 

    @foreach ($users as $user)
        @if ($loop->first)
            This is the first iteration.
        @endif

        @if ($loop->last)
            This is the last iteration.
        @endif

        <p>This is user {{ $user->id }}</p>
    @endforeach

If you are in a nested loop, you may access the parent loop's `$loop` variable via the `parent` property:

반복문이 중첩된 경우라면, 상위 반복문의 `$loop` 변수에 `parent` 속성을 통해서 액세스 할 수 있습니다:

    @foreach ($users as $user)
        @foreach ($user->posts as $post)
            @if ($loop->parent->first)
                This is first iteration of the parent loop.
            @endif
        @endforeach
    @endforeach

The `$loop` variable also contains a variety of other useful properties:

`$loop` 변수는 그 밖에도 여러가지 유용한 속성을 가지고 있습니다:

Property  | Description
------------- | -------------
`$loop->index`  |  The index of the current loop iteration (starts at 0).
`$loop->iteration`  |  The current loop iteration (starts at 1).
`$loop->remaining`  |  The iteration remaining in the loop.
`$loop->count`  |  The total number of items in the array being iterated.
`$loop->first`  |  Whether this is the first iteration through the loop.
`$loop->last`  |  Whether this is the last iteration through the loop.
`$loop->depth`  |  The nesting level of the current loop.
`$loop->parent`  |  When in a nested loop, the parent's loop variable.


속성 | 설명
------------- | -------------
`$loop->index`  |  현재 반복문의 인덱스(0 부터 시작).
`$loop->iteration`  |  현재 반복문의 횟수(1 부터 시작).
`$loop->remaining`  |  반복문의 남은 횟수.
`$loop->count`  |  반복되는 배열의 총 아이템 수.
`$loop->first`  |  현재 반복문의 첫번째 인지 확인.
`$loop->last`  |  현재 반복문의 마지막 인지 확인.
`$loop->depth`  |  중첩된 반복문의 깊이.
`$loop->parent`  |  반복문이 중첩된 경우 부모의 루프 변수.

<a name="comments"></a>
### Comments
### 주석

Blade also allows you to define comments in your views. However, unlike HTML comments, Blade comments are not included in the HTML returned by your application:

블레이드는 또한 뷰에 주석을 정의할 수 있습니다. 하지만 HTML 주석과는 다르게 블레이드 주석은 어플리케이션이 반환하는 HTML에 포함되어 있지 않습니다: 

    {{-- This comment will not be present in the rendered HTML --}}

<a name="php"></a>
### PHP
### PHP

In some situations, it's useful to embed PHP code into your views. You can use the Blade `@php` directive to execute a block of plain PHP within your template:

상황에 따라서, 뷰에서 PHP 코드 자체를 삽입하는 것이 유용할 수도 있습니다. 템플릿 안에서 블레이드의 `@php` 지시어를 사용하여 해당 블럭의 PHP를 실행 할 수 있습니다:  

    @php
        //
    @endphp

> {tip} While Blade provides this feature, using it frequently may be a signal that you have too much logic embedded within your template.

> {tip} 블레이드가 이 기능을 제공하지만, 이 기능을 너무 빈번하게 사용하는 것은 너무 많은 로직이 템플린 안에 포함되어 있다는 신호일 수 있습니다. 

<a name="including-sub-views"></a>
## Including Sub-Views
## 하위 뷰 포함하기

Blade's `@include` directive allows you to include a Blade view from within another view. All variables that are available to the parent view will be made available to the included view:

블레이드의 `@include` 지시어는 다른 뷰 내부에 하나의 블레이드 뷰를 포함할 수 있도록 해줍니다. 부모 뷰에서 볼 수 있는 모든 변수들은 포함되는 뷰에도 제공될 것입니다: 

    <div>
        @include('shared.errors')

        <form>
            <!-- Form Contents -->
        </form>
    </div>

Even though the included view will inherit all data available in the parent view, you may also pass an array of extra data to the included view:

하위에 포함하게 될 뷰는 부모 뷰의 모든 데이터를 상속하게 되지만, 하위 뷰에 데이터 배열을 직접 전달할 수도 있습니다:

    @include('view.name', ['some' => 'data'])

Of course, if you attempt to `@include` a view which does not exist, Laravel will throw an error. If you would like to include a view that may or may not be present, you should use the `@includeIf` directive:

당연하게도, `@include` 의 뷰가 존재하지 않으면 라라벨은 에러를 발생합니다. 현재 존재하지 않을 수도 있는 뷰를 포함하려는 경우에는 `@includeIf` 지시어를 사용해야 합니다:  

    @includeIf('view.name', ['some' => 'data'])

If you would like to `@include` a view depending on a given boolean condition, you may use the `@includeWhen` directive:

bool 값에 따라서 뷰 파일을 `@include` 하기를 원한다면 `@includeWhen` 지시어를 사용하면 됩니다:

    @includeWhen($boolean, 'view.name', ['some' => 'data'])

> {note} You should avoid using the `__DIR__` and `__FILE__` constants in your Blade views, since they will refer to the location of the cached, compiled view.

> {note} 블레이드 뷰에서 `__DIR__`와 `__FILE__` 를 사용하지 마십시오. 이를 사용하면 컴파일된 캐시 뷰의 경로가 반환됩니다. 

<a name="rendering-views-for-collections"></a>
### Rendering Views For Collections
### 컬렉션을 뷰에서 렌더링하기

You may combine loops and includes into one line with Blade's `@each` directive:

블레이드의 `@each` 지시어을 사용하면 반복문을 하나의 줄로 구성할 수 있습니다: 

    @each('view.name', $jobs, 'job')

The first argument is the view partial to render for each element in the array or collection. The second argument is the array or collection you wish to iterate over, while the third argument is the variable name that will be assigned to the current iteration within the view. So, for example, if you are iterating over an array of `jobs`, typically you will want to access each job as a `job` variable within your view partial. The key for the current iteration will be available as the `key` variable within your view partial.

첫번째 인자는 배열이나 컬렉션의 각 요소를 렌더링하기 위한 부분적 뷰의 이름입니다. 두번째 인자는 반복 처리하는 배열이나 컬렉션이며 세번째 인수는 뷰에서의 반복값이 대입되는 변수의 이름입니다. 예를 들어 `jobs` 배열을 반복 처리하려면 보통 부분적 뷰에서 각 과제를 `job` 변수로 접근해야 할 것입니다. 현재 반복에서의 키값은 부분적 뷰에서 `key` 변수로 접근할 수 있습니다. 

You may also pass a fourth argument to the `@each` directive. This argument determines the view that will be rendered if the given array is empty.

또한 `@each` 지시어에 네번째 인수를 전달할 수도 있습니다. 이 인자는 특정 배열이 비었을 경우 렌더링될 뷰를 결정합니다. 

    @each('view.name', $jobs, 'job', 'view.empty')

<a name="stacks"></a>
## Stacks
## 스택

Blade allows you to push to named stacks which can be rendered somewhere else in another view or layout. This can be particularly useful for specifying any JavaScript libraries required by your child views:

블레이드는 또한 다른 뷰 또는 레이아웃에서 렌더링 할 수 있도록 이름이 지정된 스택에 푸시 할 수 있습니다. 이는 특히 하위 뷰에 필요한 JavaScript 라이브러리를 지정하는 데 유용합니다: 

    @push('scripts')
        <script src="/example.js"></script>
    @endpush

You may push to a stack as many times as needed. To render the complete stack contents, pass the name of the stack to the `@stack` directive:

필요한 경우 여러번 스택에 푸쉬할 수 있습니다. 전체 스택 컨텐츠를 렌더링 하려면, 스택 이름을 `@stack` 지시어에 전달하면 됩니다:

    <head>
        <!-- Head Contents -->

        @stack('scripts')
    </head>

<a name="service-injection"></a>
## Service Injection
## 서비스 인젝션-주입

The `@inject` directive may be used to retrieve a service from the Laravel [service container](/docs/{{version}}/container). The first argument passed to `@inject` is the name of the variable the service will be placed into, while the second argument is the class or interface name of the service you wish to resolve:

`@inject` 지시어는 라라벨의 [서비스 컨테이너](/docs/{{version}}/container)에서 서비스를 반환하는 데 사용할 수 있습니다. `@inject` 지시어에 전달하는 첫번째 인자는 서비스를 할당할 변수의 이름이고, 두번째는 의존성을 해결하려는 서비스 클래스 또는 인터페이스의 이름입니다: 

    @inject('metrics', 'App\Services\MetricsService')

    <div>
        Monthly Revenue: {{ $metrics->monthlyRevenue() }}.
    </div>

<a name="extending-blade"></a>
## Extending Blade
## 블레이드 기능 확장하기

Blade allows you to define your own custom directives using the `directive` method. When the Blade compiler encounters the custom directive, it will call the provided callback with the expression that the directive contains.

블레이드에서는 `directive` 메소드를 사용하여 사용자가 고유한 지시어을 정의할 수 있습니다. 블레이드 컴파일러가 사용자가 정의한 지시어을 발견하면 지시어에 정의된 콜백 함수를 호출합니다.

The following example creates a `@datetime($var)` directive which formats a given `$var`, which should be an instance of `DateTime`:

다음의 예제는 전달된 `DateTime` 인스턴스인 `$var`의 포맷을 변경하는 `@datetime($var)` 지시어을 생성합니다: 

    <?php

    namespace App\Providers;

    use Illuminate\Support\Facades\Blade;
    use Illuminate\Support\ServiceProvider;

    class AppServiceProvider extends ServiceProvider
    {
        /**
         * Perform post-registration booting of services.
         *
         * @return void
         */
        public function boot()
        {
            Blade::directive('datetime', function ($expression) {
                return "<?php echo ($expression)->format('m/d/Y H:i'); ?>";
            });
        }

        /**
         * Register bindings in the container.
         *
         * @return void
         */
        public function register()
        {
            //
        }
    }

As you can see, we will chain the `format` method onto whatever expression is passed into the directive. So, in this example, the final PHP generated by this directive will be:

예제에서 볼 수 있듯이 이 지시어에 어떤 것이든 전달된 표현식에서 `format` 메소드를 체이닝합니다. 따라서 이 예제의 지시어의 경우에는 최종적으로 생성되는 PHP 코드는 다음과 같습니다: 

    <?php echo ($var)->format('m/d/Y H:i'); ?>

> {note} After updating the logic of a Blade directive, you will need to delete all of the cached Blade views. The cached Blade views may be removed using the `view:clear` Artisan command.

> {note} 블레이드 지시어 로직을 수정한 뒤에는, 블레이드 뷰 캐시를 삭제할 필요가 있습니다. 블레이드 뷰의 캐시는 `view:clear` 아티즌 명령어를 사용하여 제거할 수 있습니다. 