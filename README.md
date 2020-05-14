1. Add `docs` (not example, but e.g. from Cart) dir into the `/vX/api/src/main/resources/webapp` dir.

2. Replace exposed `/swagger`:

```yaml
  swagger:
    base-url: http://localhost:8080
```

with (or add) `/openapi` in `/vX/api/src/main/resources/config.yml`:

```yaml
  openapi-mp:
    enabled: true
    servlet:
      mapping: /openapi
    ui:
      enabled: true
      mapping: /openapi/ui
```

3. Enable scanning of libraries with the model classes (use Maven's `artifactId` of the module, e.g. in `/vX/lib/pom.xml`):
  
 ```yaml
  dev:
    scan-libraries:
      - SERVICE-LIB-ARTEFACT-ID
```

4. Expose `docs` through Jetty in `/vX/api/src/main/resources/webapp/WEB-INF/web.xml`:

```xml
    <servlet-mapping>
        <servlet-name>default</servlet-name>
        <url-pattern>/docs/*</url-pattern>
    </servlet-mapping>
```

5. Enable access without security to `/openapi`, `/openapi/ui` and `/docs`:

```xml
    <security-constraint>
        <web-resource-collection>
            <web-resource-name>All Access</web-resource-name>
            ...
            <url-pattern>/openapi/*</url-pattern>
            <url-pattern>/docs/*</url-pattern>
            ...
            <http-method>GET</http-method>
        </web-resource-collection>
        ...
    </security-constraint>
```

6. Use at least:

	* ```<kumuluzee.version>3.10.0-SNAPSHOT</kumuluzee.version>```
	* ```<kumuluzee-openapi-mp.version>1.2.0-SNAPSHOT</kumuluzee-openapi-mp.version>```
	* ```<kumuluzee-config-mp.version>1.4.0</kumuluzee-config-mp.version>```
	* ```<maven.compiler.source>11</maven.compiler.source>```
	* ```<maven.compiler.target>11</maven.compiler.target>```

7. In `docs` dir modify service specific data in:

	* `_coverpage.md`
	* `index.html`
	* `rapiDoc.html`
	* `swaggerUi.html`

8. In `docs` dir write content of:

	* `changelog.md`
	* `developer-guide.md`
	* `examples.yaml`
	* `getting-started.md`
	* `README.md`

9. Reuse `OASConstants` (formerly known as `OpenApiSunesisConstants`) class and make your own `OpenApiSunesisConfig` class when annotatiing `public class RestApplication extends Application`. 

10. Annotate also all resources of the `RestApplication`.

11. Preview docs by:

	* [installng](https://docsify.js.org/#/quickstart?id=quick-start) and [running](https://docsify.js.org/#/quickstart?id=preview-your-site) Docsify
	* run service and access `http://localhost:8080/docs`