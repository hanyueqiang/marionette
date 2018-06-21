## Marionette 基本概念
该文档涵盖了Marionette基本使用方式和概念。这包括*calling conventions, setting attributes, common option patterns etc*.

## 目录
Class-based Inheritance
Value Attributes
Functions Returning Values
Binding Attributes on Instantiation
Setting Options
The getOption Method
The mergeOptions Method

## Class-based Inheritance
`Backbone`和	`Marionette`利用_.extend方法实现对类的继承，例如：`Marionette.View`、`Marionette.Object`等等通过`extend`方法达到继承的目的。

在下面的示例中，我们创建了一个名为`MyView`的新的实例：

		var Mn = require('backbone.marionette');

		var MyView = Mn.View.extend({});

你也可以用JavaScript的 new 来创建`MyView`实例：

		var view = new MyView();

## Value Attributes
当我们通过extend扩展类，我们可以通过传递特定的参数来定义在对象中属性：

		var Mn = require('backbone.marionette');

		var MyView = Mn.View.extend({
			className: 'bg-success',
			template: '#template-identifier',
			regions: {	
				myRegion: '.my-region'
			},
			modelEvents: {
				change: 'removeBackground'
			},
			removeBackground: function() {
    			this.$el.removeClass('bg-success');
  			}
		});

[Live example](https://jsfiddle.net/marionettejs/k93pejyb/)	

当我们实例化`MyView`时，类名为`.bg-success`会在类名为`.my-region`的区域下创建元素。
## Functions Returning Values
我们有时需要设置一个值在等函数运行时设置，因此，`Marionette`会在实例化后函数运行时执行并返回结果：
		var Mn = require('backbone.marionette');

		var MyView = Mn.View.extend({
  			className: function() {
    			return this.model.successful() ? 'bg-success' : 'bg-error';
  			},

  			template: '#template-identifier',

  			regions: function() {
    			return {
     				 myRegion: '.my-region'
    			};
  			},

  			modelEvents: function() {
    			var wasSuccessful = this.model.successful();
    			return {
      				change: wasSuccessful ? 'removeBackground' : 'alert'
    			};
  			},

  			removeBackground: function() {
    			this.$el.removeClass('bg-success');
  			},

  			alert: function() {
    			console.log('model changed');
  			}
		});

[Live example](https://jsfiddle.net/marionettejs/nn1754fc/)

正如我们所看到，几乎所有的属性都可以动态地计算出来。在大多数情况下，`Marionette`将在实例化或首次`render`调用该函数，并在视图的整个生命周期内保存该值。
## Function Context
设置属性时，`Marionette`会把this指向你new的实例。您可以使用这个特性来确保您能够访问您的对象。
## Binding Attributes on Instantiation
在`Marionette`中，在定义类时，大多数属性可以绑定在类实例上。同时也可以绑定`events`、`triggers`、`Model`和`collections`：
		var Mn = require('backbone.marionette');

		var MyView = Mn.View.extend({
  			template: '#template-identifier'
		});

		var myView = new MyView({
  			triggers: {
    			'click a': 'show:link' //点击a链接在views上会触发'show:link'
  			}
		});

下面重新写定义triggers，例如：
		var Mn = require('backbone.marionette');

		var MyView = Mn.View.extend({
  			template: '#template-identifier',

			triggers: {
			    'click @ui.save': 'save:form'
			}
		});

		var myView = new MyView({
		  	triggers: {
		    	'click a': 'show:link'
		  	}
		});

在这个例子中，'save:form'将不再被触发，因为'show:link'的triggers属性覆盖了它。
