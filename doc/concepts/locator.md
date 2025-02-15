---
sidebar_position: 1
sidebar_label: Locator 
---
# Locator 

## Overview 
This section is about more detail and advanced usage for Clicknium Locators.
  
## Locator Settings
 Locators have the rich feature to meet different UI automation needs. 
 - Locator, UI element locator, the string(XML fragment) that used to find the elements including application information and all necessary parent nodes of the UI element, and attributes of each element.
- Locator store, the file to store locator data. The locators of the same application are stored in one locator store by default. Users can manage the locator store in Visual Studio Code or Clicnium Recorder.
- Screenshot, while recording the elements, store the screenshot together with the locator
- Attributes, users can select/deselect or edit the value of each attribute.  
---
You can open and edit the locator.
![clicknium vscode](./../img/main.png)  
UI element locator is a string, XML fragment in the following formats, with all information to locate the element.  
```
<Application .../><Uia .../>/<Uia .../>  
<Application .../><Tab .../>/<Web .../>  
```
First node `Application` contains attributes of the target application
<Application processName="notepad" filePath="notepad.exe" version="1.3" />  

| Name      | Description |
| ----------- | ----------- |
| processName      |  name of the target process   |
| filePath      |  the name of process file, can be ignored when locating element  |
| version      |  clicknium locator schema version   |  

The last node contains the attibutes of the target UI element.
The nodes between `Application` and the last node are the parent or ancestor of the target element.

The attributes in locator are used to identify the target element. The operator of attribute value is `equals` by default, and we support the following operators:
`equals`, `contains`, `startWith`, `endWith`.  
The attributes only supporting wildword search, for example `name='test?_node*`. '?' match 1 character, '*' match 0 or more characters can use `contains`, `startWith`, `endWith`.  
As Clicknium supports different automation technologies, shown as diffrent tags and attributes collection in locator schema, you can see the attributes defined in each automation technology page: [UIA](./uia.md), [web](./web.md), [Java](./java.md), [IA](./ia.md).  


## Examples
### Wildword locator
Open Edge browser, press `F12` to show developer tool sidebar, you can see the button 'Toggle device emulation'.
![sample1](./../img/locator_sample1_1.png)  
If you capture the locator of button 'Toggle device emulation', you can see the following locators.

- normal mode  
  
![sample1](./../img/locator_sample1_2.png)  
- mobile emulation mode  

![sample1](./../img/locator_sample1_3.png)  

The attribute of classname is dynamic. To stabilize identification of button 'Toggle device emulation', you can leverage wildword locator.  
![sample1](./../img/locator_sample1_4.png) 

In both modes, the button can be located successfully.
And you can know the current mode through get_property. 
```
from clicknium import clicknium as cc, ui, locator

class_name = ui(locator.msedge.bing.button_toggledeviceemulation).get_property("ClassName")
if class_name.endswith('state-off'):
    print('normal mode')
else:
    print('mobile emulation mode')
```

The following cases can be considered as wildword locator:
- the window title is dynamic, for example the title includes version string, but application version can be upgraded
- the url of the web page is dynamic
- value of the attribute is dynamic