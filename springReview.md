# spring review

## Factorybean和beanfactory有什么区别
> + beanfactory是spring的IOC的容器，用于管理bean的一个工厂
> + beanfactory是一个接口，常用实现类：ApplicationContext/xmlBeanfactory/DefaultListableBeanFactory
> + factorybean是一个特殊的bean，能生产或者修饰对象生成的工厂bean，它的实现与设计模式中的工厂模式和修饰器模式类似。

## 引入装饰器模式
> + 意图：动态地给一个对象添加一些额外的职责。就增加功能来说，装饰器模式相比生成子类更为灵活。
> + 主要解决：一般的，我们为了扩展一个类经常使用继承方式实现，由于继承为类引入静态特征，并且随着扩展功能的增多，子类会很膨胀。

## spring IOC
> + IOC控制反转是一种思想，将原本需要手动管理的对象统一由spring的IOC容器去管理，使用时可以通过注解的方式去获取对象，不需要在任何service中去new对象。
> + 

## sprin bean的生命周期

> + beanfactory通过反射??去实例化一个bean
> + 填充bean的属性 pring上下文对实例化的Bean进行配置－－也就是IOC注入
> + 如果bean实现了BeanNameAware接口，则会调用他的SetBeanName(Str)函数，此处的str就是定义的bean的id
> + 如果bean实现了BeanFactoryAware接口，会调用它的SetBeanFactory方法，参数是spring工厂自身
> + 如果实现了ApplicationContextAware接口 则会调用setApplicationContext(ApplicationContext)传入spring上下文，同样这个方式也可以实现步骤4的内容，但比4更好，因为ApplicationContext是BeanFactory的子接口，有更多的实现方法）；
> + 如果Bean实现了BeanPostProcessor接口，则调用其postProcessBeforeInitialization方法。
> + 如果Bean在Spring配置文件中配置了init-method属性会自动调用其配置的初始化方法
> + 如果有和Bean关联的BeanPostProcessors对象，则这些对象的postProcessAfterInitialization方法被调用。
> + 当销毁Bean实例时，如果Bean实现了DisposableBean接口，则调用其destroy方法。
> + 最后，如果这个Bean的Spring配置中配置了destroy-method属性，会自动调用其配置的销毁方法。
