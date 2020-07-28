# IDE 配置

## 1、IDEA

### 1、显示出RunDashboard

在`.idea`文件夹下找到`workspace.xml`在`<component name="RunDashboard">`节点添加如下配置

```xml
 <option name="configurationTypes">
      <set>
        <option value="SpringBootApplicationConfigurationType" />
      </set>
    </option>
```



