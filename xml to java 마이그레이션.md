# 스프링 프레임워크에서 스프링부트로 마이그레이션 중 발생하는 문제

## xml > java 마이그레이션 중 오류 발생
![image](https://github.com/yangju1009/DevTips/assets/145637278/7a55f693-84fa-41d7-9a72-966c3fa49c36)
Caused by: java.lang.IllegalArgumentException: Mapped Statements collection already contains value for board.select. please check mapper/boardMapper.xml
board 매퍼에서 오류 가 발생하여 확인
### boardMapper.xml
![image](https://github.com/yangju1009/DevTips/assets/145637278/6e4d475c-05f3-4494-8690-a8ae969715a0)
하지만 겹치는 id 는 존재하지 않음

# 해결 
* root-context.xml 을 RootConfig.java 로 변경하는 과정 중 문제가 발생
### RootConfig.java
```
@Bean
	public SqlSessionFactory sqlSessionFactory() throws Exception {
		SqlSessionFactoryBean sessionFactory = new SqlSessionFactoryBean();
		sessionFactory.setDataSource(dataSource());
		sessionFactory.setConfigLocation(applicationContext.getResource("classpath:/mybatis-config.xml"));
		sessionFactory.setMapperLocations(
				new PathMatchingResourcePatternResolver().getResources("classpath:mapper/*.xml"));
		return sessionFactory.getObject();

	}
```
### mybatis-config.xml
```
<mappers>
		<!-- SQL문 정의하는 파일들의 목록을 지정. -->
		<mapper resource="mapper/boardMapper.xml" />
```
setMapperLocations 와 mybatis-config.xml 의 mapper 정의 코드와 중복을 일으켜서 오류가 발생된 것으로 확인

mybatis-config.xml의 <mappers> 부분을 주석 처리하는 것으로 해결
