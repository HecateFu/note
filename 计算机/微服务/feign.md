[spring cloud-Feign使用中遇到的问题总结_牛奋lch-CSDN博客](https://blog.csdn.net/liuchuanhong1/article/details/54728681)

[java.lang.IllegalStateException: RequestParam.value() was empty on parameter 0_大话家的博客-CSDN博客](https://blog.csdn.net/qq_41057885/article/details/119884684)

```
Caused by: org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'com.hzwq.tool.feign.IEncrypFeignClient': FactoryBean threw exception on object creation; nested exception is java.lang.IllegalStateException: RequestParam.value() was empty on parameter 0
	at org.springframework.beans.factory.support.FactoryBeanRegistrySupport.doGetObjectFromFactoryBean(FactoryBeanRegistrySupport.java:176)
	at org.springframework.beans.factory.support.FactoryBeanRegistrySupport.getObjectFromFactoryBean(FactoryBeanRegistrySupport.java:101)
	at org.springframework.beans.factory.support.AbstractBeanFactory.getObjectForBeanInstance(AbstractBeanFactory.java:1680)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.getObjectForBeanInstance(AbstractAutowireCapableBeanFactory.java:1247)
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:258)
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:199)
	at org.springframework.beans.factory.config.DependencyDescriptor.resolveCandidate(DependencyDescriptor.java:277)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.addCandidateEntry(DefaultListableBeanFactory.java:1501)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.findAutowireCandidates(DefaultListableBeanFactory.java:1458)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.doResolveDependency(DefaultListableBeanFactory.java:1239)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveDependency(DefaultListableBeanFactory.java:1196)
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor$AutowiredFieldElement.inject(AutowiredAnnotationBeanPostProcessor.java:595)
	... 36 common frames omitted
Caused by: java.lang.IllegalStateException: RequestParam.value() was empty on parameter 0
	at feign.Util.checkState(Util.java:130)
	at org.springframework.cloud.openfeign.annotation.RequestParamParameterProcessor.processArgument(RequestParamParameterProcessor.java:65)
	at org.springframework.cloud.openfeign.support.SpringMvcContract.processAnnotationsOnParameter(SpringMvcContract.java:292)
	at feign.Contract$BaseContract.parseAndValidateMetadata(Contract.java:110)
	at org.springframework.cloud.openfeign.support.SpringMvcContract.parseAndValidateMetadata(SpringMvcContract.java:188)
	at feign.Contract$BaseContract.parseAndValidatateMetadata(Contract.java:66)
	at feign.hystrix.HystrixDelegatingContract.parseAndValidatateMetadata(HystrixDelegatingContract.java:47)
	at feign.ReflectiveFeign$ParseHandlersByName.apply(ReflectiveFeign.java:154)
	at feign.ReflectiveFeign.newInstance(ReflectiveFeign.java:52)
	at feign.Feign$Builder.target(Feign.java:251)
	at org.springframework.cloud.openfeign.HystrixTargeter.target(HystrixTargeter.java:57
	at org.springframework.cloud.openfeign.FeignClientFactoryBean.loadBalance(FeignClientFactoryBean.java:238)
	at org.springframework.cloud.openfeign.FeignClientFactoryBean.getTarget(FeignClientFactoryBean.java:267)
	at org.springframework.cloud.openfeign.FeignClientFactoryBean.getObject(FeignClientFactoryBean.java:247)
	at org.springframework.beans.factory.support.FactoryBeanRegistrySupport.doGetObjectFromFactoryBean(FactoryBeanRegistrySupport.java:169)
	... 47 common frames omitted
```

