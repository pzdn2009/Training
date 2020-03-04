# Utils

java常用的工具类库积累。

* SerializationUtils
* WordUtils
* ClassUtils

其他： HttpUtils、DownloadManagerPro、ShellUtils、PackageUtils、PreferencesUtils、JSONUtils、FileUtils、ResourceUtils、StringUtils、ParcelUtils、RandomUtils、ArrayUtils、ImageUtils、ListUtils、MapUtils、ObjectUtils、SerializeUtils、SystemUtils、TimeUtils。 StringEscapeUtils，NumberUtils，CharSetUtils

```java
private final class BuildDemo {    
        String name;    
        int age;    

        public BuildDemo(String name, int age) {    
            this.name = name;    
            this.age = age;    
        }    

        public String toString() {    
            ToStringBuilder tsb = new ToStringBuilder(this, ToStringStyle.MULTI_LINE_STYLE);    
            tsb.append("Name", name);    
            tsb.append("Age", age);    
            return tsb.toString();    
        }    

        public int hashCode() {    
            HashCodeBuilder hcb = new HashCodeBuilder();    
            hcb.append(name);    
            hcb.append(age);    
            return hcb.hashCode();    
        }    

        public boolean equals(Object obj) {    
            if (!(obj instanceof BuildDemo)) {    
                return false;    
            }    
            BuildDemo bd = (BuildDemo) obj;    
            EqualsBuilder eb = new EqualsBuilder();    
            eb.append(name, bd.name);    
            eb.append(age, bd.age);    
            return eb.isEquals();    
        }    
    }
```

\`\`\`java StringUtils.repeat\("\*", 50\)

