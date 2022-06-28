工具版本

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
    <version>2.2.6.RELEASE</version>
</dependency>
```

### PropertySource

- MutablePropertySources

```java
public abstract class PropertySource<T> {

	protected final Log logger = LogFactory.getLog(getClass());

	protected final String name;

	protected final T source;


	public PropertySource(String name, T source) {
		Assert.hasText(name, "Property source name must contain at least one character");
		Assert.notNull(source, "Property source must not be null");
		this.name = name;
		this.source = source;
	}

	@SuppressWarnings("unchecked")
	public PropertySource(String name) {
		this(name, (T) new Object());
	}

	public String getName() {
		return this.name;
	}

	
	public T getSource() {
		return this.source;
	}


	public boolean containsProperty(String name) {
		return (getProperty(name) != null);
	}


	@Nullable
	public abstract Object getProperty(String name);


	@Override
	public boolean equals(@Nullable Object other) {
		return (this == other || (other instanceof PropertySource &&
				ObjectUtils.nullSafeEquals(this.name, ((PropertySource<?>) other).name)));
	}


	@Override
	public int hashCode() {
		return ObjectUtils.nullSafeHashCode(this.name);
	}


	@Override
	public String toString() {
		if (logger.isDebugEnabled()) {
			return getClass().getSimpleName() + "@" + System.identityHashCode(this) +
					" {name='" + this.name + "', properties=" + this.source + "}";
		}
		else {
			return getClass().getSimpleName() + " {name='" + this.name + "'}";
		}
	}


	public static PropertySource<?> named(String name) {
		return new ComparisonPropertySource(name);
	}


	public static class StubPropertySource extends PropertySource<Object> {

		public StubPropertySource(String name) {
			super(name, new Object());
		}

		/**
		 * Always returns {@code null}.
		 */
		@Override
		@Nullable
		public String getProperty(String name) {
			return null;
		}
	}

	static class ComparisonPropertySource extends StubPropertySource {

		private static final String USAGE_ERROR =
				"ComparisonPropertySource instances are for use with collection comparison only";

		public ComparisonPropertySource(String name) {
			super(name);
		}

		@Override
		public Object getSource() {
			throw new UnsupportedOperationException(USAGE_ERROR);
		}

		@Override
		public boolean containsProperty(String name) {
			throw new UnsupportedOperationException(USAGE_ERROR);
		}

		@Override
		@Nullable
		public String getProperty(String name) {
			throw new UnsupportedOperationException(USAGE_ERROR);
		}
	}

}
```



### ObjectUtils

### MergedAnnotations

### ClassUtils

- ##### isPresent

  >

  ```java
  public static boolean isPresent(String className, @Nullable ClassLoader classLoader) {
      try {
          forName(className, classLoader);
          return true;
      }
      catch (IllegalAccessError err) {
          throw new IllegalStateException("Readability mismatch in inheritance hierarchy of class [" +
                                          className + "]: " + err.getMessage(), err);
      }
      catch (Throwable ex) {
          // Typically ClassNotFoundException or NoClassDefFoundError...
          return false;
      }
  }
  ```

  

### LinkedMultiValueMap

> 单个key对应一条链表集合



### StringUtils

- **commaDelimitedListToStringArray**

  > 将逗号分隔转成数组

  ```java
  public static String[] commaDelimitedListToStringArray(@Nullable String str) {
  		return delimitedListToStringArray(str, ",");
  }
  ```

  









### UrlResource

```java
UrlResource resource = new UrlResource(url);
Properties properties = PropertiesLoaderUtils.loadProperties(resource);
```



### PropertiesLoaderUtils

## 关于资源获取

```java
package cn.test1;

import org.junit.Test;
import org.springframework.core.GenericTypeResolver;
import org.springframework.core.env.AbstractPropertyResolver;

import java.io.IOException;
import java.net.URL;
import java.util.Enumeration;

public class Demo2 {

    @Test
    public void test1() throws IOException {

        //获取当前项目绝对路径
        final URL AbsolutePath = Demo2.class.getClassLoader().getResource("");
        System.out.println("当前绝对路径:" + AbsolutePath);

        //获取依赖路径
        final Enumeration<URL> resource = Demo2.class.getClassLoader().getResources("META-INF/spring.factories");
        while (resource.hasMoreElements()) {
            System.out.println(resource.nextElement());
        }

    }

}
```

