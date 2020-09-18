# ros_dynamic_reconfigure_template

## The cfg File Explained

```
PACKAGE = "dynamic_tutorials"

from dynamic_reconfigure.parameter_generator_catkin import *
```
This first lines are pretty simple, they just initialize ros and import the parameter generator.
```
gen = ParameterGenerator()
```
Now that we have a generator we can start to define parameters. The add function adds a parameter to the list of parameters. It takes a few different arguments:

- name - a string which specifies the name under which this parameter should be stored
- paramtype - defines the type of value stored, and can be any of int_t, double_t, str_t, or bool_t
- level - A bitmask which will later be passed to the dynamic reconfigure callback. When the callback is called all of the level values for parameters that have been changed are ORed together and the resulting value is passed to the callback.
- description - string which describes the parameter
- default - specifies the default value
- min - specifies the min value (optional and does not apply to strings and bools)
- max - specifies the max value (optional and does not apply to strings and bools)\
```
gen.add("int_param",    int_t,    0, "An Integer parameter", 50,  0, 100)
gen.add("double_param", double_t, 0, "A double parameter",    .5, 0,   1)
gen.add("str_param",    str_t,    0, "A string parameter",  "Hello World")
gen.add("bool_param",   bool_t,   0, "A Boolean parameter",  True)
```
These lines simply define more parameters of the different types.

```
size_enum = gen.enum([ gen.const("Small",      int_t, 0, "A small constant"),
                       gen.const("Medium",     int_t, 1, "A medium constant"),
                       gen.const("Large",      int_t, 2, "A large constant"),
                       gen.const("ExtraLarge", int_t, 3, "An extra large constant")],
                     "An enum to set size")

gen.add("size", int_t, 0, "A size parameter which is edited via an enum", 1, 0, 3, edit_method=size_enum)
```
Here we define an integer whose value is set by an enum. To do this we call gen.enum and pass it a list of constants followed by a description of the enum. Now that we have created an enum we can now pass it to the generator. Now the param can be set with "Small" or "Medium" rather than 0 or 1.
```
exit(gen.generate(PACKAGE, "dynamic_tutorials", "Tutorials"))
```
The last line simply tells the generator to generate the necessary files and exit the program. The second parameter is the name of a node this could run in (used to generate documentation only), the third parameter is a name prefix the generated files will get (e.g. "<name>Config.h" for c++, or "<name>Config.py" for python
  
## Using the cfg File
In order to make this cfg file usable it must be executable, so lets use the following command to make it excecutable
```
chmod a+x cfg/Tutorials.cfg
```
Next we need to add the following lines to our CMakeLists.txt. For Groovy and above
```
#find_package(catkin REQUIRED dynamic_reconfigure)
generate_dynamic_reconfigure_options(
  cfg/Tutorials.cfg
  #...
)
```
## References
- http://wiki.ros.org/dynamic_reconfigure/Tutorials/HowToWriteYourFirstCfgFile
- http://wiki.ros.org/dynamic_reconfigure/Tutorials/SettingUpDynamicReconfigureForANode%28cpp%29
