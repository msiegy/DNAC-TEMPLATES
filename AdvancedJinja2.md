# Advanced Jinja2 [![published](https://static.production.devnetcloud.com/codeexchange/assets/images/devnet-published.svg)](https://developer.cisco.com/codeexchange/github/repo/kebaldwi/DNAC-TEMPLATES)
This section will describe the various advanced templating techniques used to make a powerful script out of normal CLI command scripts which are used by organizations on IOS devices around the world. This section builds on the previous sections and is an attempt to demystify the hows and to bring clarity on what is truely possible. While it is possible to take a CLI script for one device and create a template for one device at a time, that would leave us with a lot of templates and make it harder to make changes on an ongoing basis. Using the techniques below will allow us to deploy equipment with scripts which can be reused, allowing us to keep configurations similar for conformity reasons but also to reduce the number of places where changes would have to be made. 

Below will be examples of various use cases that could be implemented.

### Parsing Integers from String Variables
In the following example a variable is bind to DNA Centers database and a value is called for the Native Vlan. As all data within the database is essentially in string format, if we wish to use it with some mathematics to calculate other values, we would first need to change it from a string to a integer.

In order to acomplish this we need to use this example. We will use the *int* modifier to parse the bind variable to an integer variable using set notation.

```j2
   {% set native_vlan = native_vlan_bind | int %}
   {% set data_vlan = native_vlan + 10 %}
```

### Working with Arrays or Ordered Lists
To create an array we have to perform the following. First we need to concatenate the values into a variable with some kind of delimiter. Then using the delimiter we can split the values into separate elements within the array and call them separately through a loop.

```j2
   {% set StackPIDs = ProductID.split(",") %}
   !
   {% for Switch in StackPIDs %}
        {% set PortCount = Switch.replaceAll('^.*([2|4][4|8]).*','$1') %}
   {% endfor %}
```
Another way we may work with arrays is to use the append operator. We will add this to the previous block of code and the method would look like this. In order to use the Append method we need an empty array. Then we append into the array with each iteration placing only the *24* or *48* from the Product ID.

```j2
   {% set PortTotal = [] %}
   {% set StackPIDs = ProductID.split(",") %}
   !
   {% for Switch in StackPIDs %}
        {% set PortCount = Switch.replaceAll('^.*([2|4][4|8]).*','$1') %}
        {% do PortTotal.append(PortCount) %}
   {% endfor %}
```

We can utilize other methods like length but in the next addition we build out ports for the switch based on ranges. Take note of the use of the loop.index variable.

```j2
   {% set PortTotal = [] %}
   {% set StackPIDs = ProductID.split(",") %}
   !
   {% for Switch in StackPIDs %}
        {% set PortCount = Switch.replaceAll('^.*([2|4][4|8]).*','$1') %}
        {% do PortTotal.append(PortCount) %}
   {% endfor %}
   !
   {% for SwitchPort in PortTotal %}
       interface {{ loop.index }}/0/1 - {{ SwitchPort }}
         desc test
   {% endfor %}
```

### Working with Stacks of 9300/9200 and Powerstacking
One area we need to address is how to effectively deal with stacking 9300's and how to deal with a stack of 8 switches where a powerstack only allows 4. Although not supported by TAC this is supported from a platform point of view. Essentially you would build the data stack of 8 switches, and then build two powerstacks of four switches in each. In the following example I share is based on velocity code which was co-written by Josh Bronikowski. 

In order to acomplish this we need to first identify how many switches are in the stack... please use this example. 

```j2
   {% set StackPIDs = ProductID.split(",") %}
   {% set StackMemberCount = StackPIDs|length %}
```
Then we need a logical construct which iterates through each switch setting not only the priority correctly but also setting the powerstack correctly.

```j2
   {% if StackMemberCount > 1 %}
      stack-power stack Powerstack1
      mode redundant strict
      {% if StackMemberCount > 4 %}
         stack-power stack Powerstack2
         mode redundant strict
      {% endif %}
      {% for Switch in range(0,StackMemberCount,1) %}
         {% if loop.index <= (StackMemberCount/2|round('ceil')) %}
            stack-power switch {{ loop.index }}
            stack Powerstack1
         {% elif loop.index > (StackMemberCount/2|round('ceil')) %}
            stack-power switch {{ loop.index }}
            stack Powerstack2
         {% endif %}
         {{ loop.index }}
      {% endfor %}
      #MODE_ENABLE
      #MODE_END_ENABLE
      #MODE_ENABLE
      {% for Switch in range(0,StackMemberCount,1) %}
         {% if loop.index == 1 %}
            switch {{ loop.index }} priority 10
         {% elif Switch == 2 %}
            switch {{ loop.index }} priority 9
         {% else %}
            switch {{ loop.index }} priority 8
         {% endif %}
      {% endfor %}
      #MODE_END_ENABLE
   {% endif %}
```
Explained here...
1. The code shared will run only if the number of switches in the stack is found to be greater than 1. This means that stackpower is only configured on stacks of two or more switches. 

```j2
   {% if StackMemberCount > 1 %}
```

2. The next step is to correctly set the number of powerstack required. If the number of switches exceeds 4 then we need two powerstacks set up.

```j2
   stack-power stack Powerstack1
   mode redundant strict
   {% if StackMemberCount > 4 %}
      stack-power stack Powerstack2
      mode redundant strict
   {% endif %}
```

3. The next step is to iterate through the switches in the stack setting the stackpower appropriately for each switch and adding them to the correct powerstack 

```j2
   {% for Switch in range(0,StackMemberCount,1) %}
      {% if loop.index <= (StackMemberCount/2|round('ceil')) %}
         stack-power switch {{ loop.index }}
         stack Powerstack1
      {% elif loop.index > (StackMemberCount/2|round('ceil')) %}
         stack-power switch {{ loop.index }}
         stack Powerstack2
      {% endif %}
      {{ loop.index }}
   {% endfor %}
```
4. Lastly, we will set the switch priority appropriately on each switch for master and standby, and then for the remaining switches within the stack so that switch numbering matches the priority levels.

```vtl
   {% for Switch in range(0,StackMemberCount,1) %}
      {% if loop.index == 1 %}
         switch {{ loop.index }} priority 10
      {% elif Switch == 2 %}
         switch {{ loop.index }} priority 9
      {% else %}
         switch {{ loop.index }} priority 8
      {% endif %}
   {% endfor %}
```

### Working port counts within Catalyst 9k
One area we need to address is how to effectively deal configuring multiple ports. For this we will make use of multiple concepts as we build out this use case.

First we would need to identify how many ports we have on each switch or linecard, and how many linecards or switches within the stack or chassis there are. For this example we are using a stack of 9300's, but you can build this for 9400's.

We set up variables to track in Array format the number of ports per switch.

```j2
   {% set PortTotal = [] %}
   {% set StackPIDs = ProductID.split(",") %}
   {% for Switch in StackPIDs %}
        {% set PortCount = Switch.replaceAll('^.*([2|4][4|8]).*','$1') %}
        {% do PortTotal.append(PortCount) %}
   {% endfor %}
```

The next step would be to build macros and vlans to configure the various ports.

```vtl
   vlan {{data_vlan_number}}
     name {{site_code}}-Employees
   vlan {{voice_vlan_number}}
     name {{site_code}}-Voice
   vlan {{iot_vlan_number}}
     name {{site_code}}-IOT
   vlan {{guest_vlan_number}}
     name {{site_code}}-Guests
   vlan {{ap_vlan_number}}
     name {{site_code}}-AP
   
   {% macro uplink_interface() %}
       switchport trunk allowed vlan add {{voice_vlan_number}},{{iot_vlan_number}},{{guest_vlan_number}},{{ap_vlan_number}}     
   {% endmacro %}
   
   {% macro access_interface () %}
      description base port config
      switchport mode access
      switchport access vlan {{data_vlan_number}}
      switchport voice vlan {{voice_vlan_number}}
      spanning-tree portfast
      spanning-tree bpduguard enable
   {% endmacro %}
   
   {% for SwitchPort in PortTotal %}
        interface range gi {{ loop.index }}/0/1 - {{ SwitchPort }}
          {{ access_interface() }}
   {% endfor %}
   
   interface portchannel 1
    {{ uplink_interface() }}
```

While this would configure the access ports, and modify the port channel for upstream connectivity we could do more to automate we will look at that next.

If you found this repository or any section helpful please fill in comments and [give feedback](https://app.smartsheet.com/b/form/f75ce15c2053435283a025b1872257fe) on how it could be improved.

Special mention to: https://jinja.palletsprojects.com/en/3.0.x/templates as examples and extrapolations were made using this documentation.