# Bean的作用域

* **singleton**。在整个Spring IoC 容器中，使用 singleton 定义的Bean将只有一个实例。
* **prototype**。原型模式,每次通过容器的getBean 方法获取prototype定义的Bean 时,都将产生一个新的Bean实例
* **request**。对于每次HTTP请求，使用request定义的Bean都将产生一个新的实例，每次HTTP请求都将产生不同的Bean实例,该作用域仅在给予**web的Spring ApplicationContext**情形下有效
* **session**。对于每次HTTP Session ,使用session定义的Bean都将产生一个新实例，该作用域仅在给予**web的Spring ApplicationContext**情形下有效
* **global session**。每个全局得HTTP Session对应一个Bean实例，该作用域仅在给予**web的Spring ApplicationContext**情形下有效

Spring容器负责跟踪监视Bean实例的状态，负责维护Bean实例的生命周期行为。

如果不指定Bean的作用域，Spring默认使用singleton作用域。

