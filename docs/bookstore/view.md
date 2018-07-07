## Marionette.View

`View` 表示用模板显示的视图。通常和`Backbone.Model` `Backbone.Collection`搭配使用，或者不需要任何搭配。

`View` 还可以用于构建应用结构布局-可以通过`regions`属性轻松地嵌套多个`views`。

**注意:** 从 `Martonette v3.X`版本开始，`Marionette.View`取代`Marionette.LayoutView`和`Marionette.ItemView` .

## 目录

- Rendering a Template

- Managing an Existing Page

- Laying out Views - Regions

	- Class Definition

		- Specifying 'regions' as function

		- Regions on View Instantiation

	- Managing Sub-views

		- Showing a view

		- Accessing a child view

- Organizing your View

- Events

## Rendering a Template

`Marionette View`是一个强大的渲染页面方式，给定模板，将从模板中创建`HTML`，可以渲染`model`中数据以及任何信息。

如果你想给`views`添加额外的方法，最好在`onBeforeRender`或`onRender 中添加你的逻辑。

当你`render`一个模板，要在`view`中设置`template`属性：

	var Mn = require('backbone.marionette');
	var _ = require('underscore');

	var myView = Mn.View.extend({
		tagNmae：'h1',
		template: _.template('Contents')
	});

	var myView = new MyView();
	myView.render();

关于如何`render`模板，请看 Template 文档

## Managing an Existing Page

`Marionette`能够管理预生成的页面——静态页面或服务器生成页面，并将它们视为来自`Marionette`。

	var Mn = require('backbone.marionette');
	
	var MyView = Mn.View({
	  el: '#base-element',
	  template: false
	});

	
`Marionette View` 通常被用于在事件触发或者UI操作完后展示出来，这之前`view`是没有模板的，我们可以在`region`开始展示'view' 时，通过监听`before:render`事件和`render`事件被调用到时 渲染我们要用的`template`	

## Laying out Views - Regions (布局试图 - Regions)
`Marionette.View`允许使用`regions`来管理试图结构，`regions`就像一个钩子一样，在`View`中显示`view`，控制显示和隐藏，对子`View`事件起作用。

本节只介绍基础，了解更多关于`Region`,看下面 `Region` 文档。

`Regions`在其他`View`渲染页面是互不影响的，这对独立渲染一块区域非常有用，在数据更新时不会重新渲染整个屏幕。

`Regions`可以定义类时添加，也可以通过`addRegion`添加。

当你给`View`扩展属性方法时，在`Regions`属性添加指定选择器，`new View`会被渲染出来。

	var Mn = require('backbone.marionette');

	var MyView = Mn.View.extend({
	    template: '#tpl-view-with-regions',
	
	    regions: {
	        firstRegion: '#first-region',
	        secondRegion: '#second-region'
	    }
	});

模板如下：

	<script type="x-template/underscore" id="tpl-view-with-regions">
	  <div id="first-region"></div>
	  <div id="second-region"></div>
	  <div id="third-region"></div>
	</script>

当我们在`Region`中显示视图时，在`#first-region`和`#second-region`会被替换我们要`show`的内容，这个`Region hash`是一个`jQuery`选择器，任何`jQuery`选择器语法都可以。

## Managing Sub-views （管理子`View`）

`View`提供了接口管理子`view`，`showChildView`和`getChildView`

## Showing a view (展示一个`View`)

在一个区域展示一个`view`,只需调用`showChildView(region, view)`，这将会渲染视图层`HTMl`，并且放在`Region`对应`Dom`下。

	var Mn = require('backbone.marionette');
	var SubView = require('./subview');
	
	var MyView = Mn.View.extend({
	  template: '#tpl-view-with-regions',
	
	  regions: {
	    firstRegion: '#first-region'
	  },
	
	  onRender: function() {
	    this.showChildView('firstRegion', new SubView());
	  }
	});

## Accessing a child view (访问子view)

访问视图的子视图，使用`getChildView(region)`方法。这将返回当前正在该`region`的视图实例，或`null`：

	var Mn = require('backbone.marionette');
	var SubView = require('./subview');
	
	var MyView = Mn.View.extend({
	  template: '#tpl-view-with-regions',
	
	  regions: {
	    firstRegion: '#first-region'
	  },
	
	  onRender: function() {
	    this.showChildView('firstRegion', new SubView());
	  },
	
	  onSomeEvent: function() {
	    var first = this.getChildView('firstRegion');
	    first.doSomething();
	  }
	});

如果没有`view`，返回`null`。