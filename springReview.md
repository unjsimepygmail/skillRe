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
