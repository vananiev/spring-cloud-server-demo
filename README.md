Demo project for [spring-cloud/spring-cloud-config#1989](https://github.com/spring-cloud/spring-cloud-config/issues/1989)

To run application use
```shell
./mvnw spring-boot:run -Dspring-boot.run.jvmArguments="-Dspring.cloud.bootstrap.enabled=true"
```
or use Idea configuration 'SpringCloudServerDemoApplication'.

`cloud-conf/app/master/application.yml` imports 2 files
```yaml
spring.config.import:
  - file1.yml
  - dir/file2.yml
```
Console output is
```
[           main] o.s.c.c.s.e.NativeEnvironmentRepository  : Adding property source: Config resource 'file [cloud-conf\app\master\file1.yml]' via location 'file1.yml'
[           main] o.s.c.c.s.e.NativeEnvironmentRepository  : Adding property source: Config resource 'file [cloud-conf\app\master\application.yml]' via location 'cloud-conf/app/master/'
[           main] b.c.PropertySourceBootstrapConfiguration : Located property source: [BootstrapPropertySource {name='bootstrapProperties-file:cloud-conf\app\master\file1.yml'}, BootstrapProper
tySource {name='bootstrapProperties-file:cloud-conf\app\master\application.yml'}]
```
So Spring Cloud Config Server ignores `cloud-conf\app\master\dir\file2.yml` import.

Second check may be done by
```
GET http://localhost:8080/app/master
```
Response excludes also `file2.yml`
```json
{
    "name": "app",
    "profiles": [ "master" ],
    "label": null,
    "version": null,
    "state": null,
    "propertySources":
    [
        {
            "name": "file:cloud-conf\\app\\master\\file1.yml",
            "source":
            {
                "b": 1
            }
        },
        {
            "name": "file:cloud-conf\\app\\master\\application.yml",
            "source":
            {
                "a": 0,
                "spring.config.import[0]": "file1.yml",
                "spring.config.import[1]": "dir/file2.yml"
            }
        }
    ]
}
```