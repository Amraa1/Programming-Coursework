# **Robots Delivery Simulation**

**Version: 4.0**

This is my university coursework for Python Programming and Software Implementation module.

**Date: Nov 2020**

# **Coursework Description**

There are a set of tasks to be completed ecosystem. You should design and implement the code necessary to accomplish the task.

The tasks are listed [here](#courseworks-stages)

## **The Ecosystem Object Definition**

The ecosystem is a virtual environment in a virtual universe in which virtual robots are created by the **_Chief Robot Programmer_** (CRP). The ecosytem has 'rules' which, if the robots do not follow, they can come to a premature demise. It is up to the CRP to programme their robots to follow system rules. A well programmed robot colony can thrive and prosper, if it is poorly programmed it may not perform so well.

There are a number of system indicators which measure the health of the ecosystem.

During the lifetime of the universe the ecosystem might evolve. This means the ecosystem rules change. Robots must therefore evolve or adapt to their new environment. The CRPs must be _agile programmers_.

A major deadline exists on the **12-January-2021**. There will be a major conference of the parties or **COP27** meeting to see whether your ecosystem is on track to meet targets and deliver a sustainable Robot ecosystem which is operating in harmony with its environment by creating a **circular economy**.

## **Ecosystem Documentation**

### **Policies**

---

The Ecosystem system sets a standard for design policies which all CRPs must adhere too.

-   For security **only** use instance variables should be use and class variables should be deprecated, i.e. those defined and assigned in the constructor method.

-   Robot Classes must be self contained within their own code cells for clarity of purpose and assessment, commented as appropriate. Instantiation and registration testing of classes should be separate

-   Where appropriate CRPs should augment code cells comments with text cells to document and explain designs for third parties and assessment in the final submission.

### **Ecosystem Rules**

_The current version rules are as follows_

---

Rules are only enforced when an ecosystem.update() call is made. All attributes are inspected, and values and changes are validated.

**Registerable Objects**

Only objects with the following (case sensitive) class names can be registered.

```
 Robot
 Droid
 Drone
```

Attempts to register anything fails. Registration failure wastes ecosystem finite resources.

**Required Attributes**
Objects must have the required attributes as documented in the Robot Class Specifications. Absence of required attributes causes registration failure. Registration failure wastes ecosystem finite resources!

**Read Only**

You can implement, in your Robots any of the attributes in the Robot attributes tables. A large number are read only. You can only set the value to the default value prior to registering a new robot with the ecosystem. Attempts to change a read-only values cause damage. Robots accrue 1 damage point per change attempt.

**Speed**

Robots must not move more than the following increments between updates in the $x$, $y$ and $z$ directions. You can regard this as a nominal maximum speed, though of course moving diagonally is permitted and this increases the speed by factor of $\sqrt{2}$ (if $\Delta x=1$ and $\Delta y = 1$). This is permitted. The main point is to adhere to permitted $x$, $y$ or $z$ increments.

|       | 𝜟x  | 𝜟y  | 𝜟z  |
| :---: | :-: | :-: | :-: |
| Robot |  1  |  1  |  0  |
| Droid |  2  |  2  |  0  |
| Drone |  3  |  3  |  1  |

Robots and Droids (non-[volitants](https://en.wiktionary.org/wiki/volitant)) can never move in the $z$ axis. They cannot fly. Furthermore, for Drones, (volitants) the following implicit rule applies:

| 𝜟x  | 𝜟y  | 𝜟z  | $\Rightarrow$ z |
| :-: | :-: | :-: | :-------------: |
|  0  |  0  |  1  |        0        |
|  3  |  3  |  1  |       >0        |

In other words Drones can only move in the air. If they are on the ground they cannot move, unless $\Delta z>0$ (upwards). Note that if they are taking off, or landing, such that either the start point or end point (but not both), is $z=0$ then they can move laterally too. I.e. vertical take offs are not mandated by this rule.

If a robot speeds the move is cancelled. Speeding causes damage of **1 damage point** per attempted speeding incident (i.e. regardless of the number of vectors involved).

**Respecting Ecosystem Boundaries**

-   Ecosystem boundaries in all dimensions must be respected. The limits are documented in the section **Ecosystem>Globals**:

    > #Ecosystem Boundaries
    >
    > default_width = 80
    >
    > default_height = 40
    >
    > default_altitude = 5

-   The lower limits are all 0.

        **Note** do not _hardwire_ boundaries when programming to respect this rule - you should always use variable names in case boundaries change in a future ecosystem release.

    A robot can _sit_ on the boundary. A Robot coordinate must not go negetive, or greater than the boundary limit. Failure to respect boundaries causes immediate malfunction of a robot. Its status is immediately set to `broken`.

An attempt to move beyond the system boundaries results in severe damage amounting to **the maximum permitted damage points**. Therefore it will break down!

**Robot Status**
A robot has one of three statuses#

> on
>
> off
>
> broken

The default status for a new Robot is `on`. A robot's status is set to broken if:

-   It runs out of charge (`soc = 0`)
-   It gets the maximum permitted damage points
-   The robot goes beyond the ecosystem boundary.

### **Ecosystem Interface**

---

The **ecosysystem class** provides an interface with a constructor method, a number of public **methods** and **properties** which are important for the CRP.

**Constructor**

There are no arguments required nor accepted by the constructor

> ecosystem = Ecosystem()

**Public properies**

> robots - returns a `list` of all registered robot objects (read only)
>
> registry - returns a dictionary which is full register of all registered robots. It is a dictionary of individual robot register as the value, and the robot object id() value as the key. A robot register contains a copy a Robot's attributes and is a valid data point for the chart. The method can accept key word arguments consisting of a robot attributes and an expected values in order to filter the registry. Thus
> ` ecosystem.registry(kind = "Drone")`
> will filter the registry for Drone objects only.
>
> duration - returns an integer which is the duration of the ecosystem in hours (default 24) (read/write). Use by the `stop` property.
>
> hour - returns an integer for the current hour of operation. Used by the stop property and also for calculating the age of robots (read only)
>
> stop - return a boolean true value if `hour >= duration` (read only)
>
> count_operational - returns an integer which is the count of robots with `status = 'on'` (read only)
>
> messages - returns a formatted list of all the fifo message stack (read only)
>
> message - returns a tuple message in the fifo message stack, or false if none is available. (read/write)
>
> has_message - returns true if there are unread messages in the message stack
>
> display_on_update - boolean to determine of the robot arena is auto-refreshed on update

**Methods**

> display(\*\*kwargs) - displays the robot arena and render the robot objects
>
> > arguments: kwargs - see chart
>
> register(robot) - registers a robot to participate in ecosystem activities
>
> > robot: a valid robot object class/subclass
>
> update(\*\*kwargs) - updates the robot register with new values for all attributes, validates permitted changes and makes necessary updates to system variables. If display_on_update is true the robot arena is updated.
>
> arguments: kwargs are passed to the display method- see chart

### **Display**

---

The display is updated everytime the ecosystem is updated if the ecosystem attribute `display_on_update = True`. This can be set to False to perform rapid simulations.

You can also call display directly for test purposes which bypasses Ecosystem updating, so you can visualise the results of robot programming without ecosystem constraints!

Calling display

> display (markers, \*\*kwargs)
>
> arguments:
>
> markers: a list of ecosystem robot registers.
>
> kwargs: (keyword arguments)
>
> > width - integer, width of chart in cm default = 30
> >
> > height - integer, height of chart in cm default = 15
> >
> > pause - integer, delay time in ms after displaying chart default = 250
> >
> > title - string, text displayed on chart default = 'Ecosystem Display'
> >
> > clear - boolean, clears code cell output area prior to display (default = True)
> >
> > hour - ecosystem time in hours (default = 12)
> >
> > annotations - list of attributes to display in a point annotation (default = none).

## **Robot Documentation**

### **Robot Class Specifications**

---

This section defines the _**interface**_ for your Robots. For our purpose this means instance variables and methods.

> > _for security purposes CRPs must **only** use instance variables, i.e. those defined and assigned in the constructor method (see ecosystem policies)._

-   **Inheritance** - Robot is the parent class from which all other robot kinds inherit. Each class can have it's own constructors and methods and/or you can use [inheritance and override methods](https://www.w3schools.com/python/python_inheritance.asp) as documented in **JNBV08**

-   **Attributes** are documented for each class in the attribute tables below. Attribute must be assigned in the constructor class. You will note the table lists **required**, **recommended** and **optional** attributes. These differ as follows:

    -   Required - each Robot class **_must_** have the **required** attributes. If these _instance variables_ do not exist in the class the ecosystem will fail to register the robot objects.
    -   Recommended - this variables are recommended because having their values to hand in your objects might help make appropriate decisions when operating in the ecosystem.
    -   Optional - these may be useful and/or become recommended or required in later stages.

    -   When assigning assigning the variables in the constructor class you must respect the data validation rules prior to registering the robot with the ecosystem. See Ecosystem Rules for more information. Specifically:
        -   Read only attributes must be assigned the given default.
        -   Other attributes - these have set validation rules which must be adhered to.
        -   To get started, using the default values for **all** Robot attributes is recommended.

-   **Methods** Methods must be designed so that a robot can achieve desired objectives. These are discussed below under Robot Methods.

### **Attribute Tables**

---

All Robot kinds should be furnished with instance variables (which here we also call attributes) in the constructor method for the class, using the self._atttribute_name_ to assign the variable to the default value.

Drones and Droids have the same variable requirements as Robots, but some of their defaults, (and validations) are different as given in the tables below.

Variables given a 'Read Only' designation must, if used, be assigned the listed default value.

_**Note** the validation for co-ordinates and velocity are identical to the maximum vector velocities. This means the change in co-ordinates, and velocity must respect the maximum velocity values. Co-ordinates are further validated by the requirement that they must within or on the system boundary. See ecosystem rules for more information._

**Robot Class Attributes**

| required    | name        | default   | validation  | datatype | description                      |
| ----------- | ----------- | --------- | ----------- | -------- | -------------------------------- |
| required    | kind        | Robot     | Read Only   | string   | robot class type                 |
| required    | coordinates | [0, 0, 0] | [1, 1, 0]   | list     | x, y, z location of a robot      |
| required    | max         | [1, 1, 0] | Read Only   | list     | maximum x, y, z velocity         |
| required    | velocity    | [0, 0, 0] | [1, 1, 0]   | list     | current x, y, z velocity         |
| required    | status      | on        | Read Only   | string   | Set to on, off or broken         |
| required    | activity    | idle      | activities  | string   | determines activity of robot     |
| required    | name        | id        | [2, 20]     | string   | named of robot                   |
| recommended | target      | None      | none        | list     | x, y, z of a target destination  |
| recommended | age         | 0         | Read Only   | integer  | age of robot in hours            |
| recommended | active      | 0         | Read Only   | integer  | active hours of robot            |
| recommended | serviced    | 0         | Read Only   | integer  | age of robot at last service     |
| recommended | soc         | 600       | Read Only   | integer  | state of charge of battery       |
| recommended | capacity    | 600       | Read Only   | integer  | energy capacity of robot battery |
| recommended | service     | 0         | Read Only   | integer  | service points accrued by robot  |
| recommended | damage      | 0         | Read Only   | integer  | damage points accrued by robot   |
| optional    | on_arena    | 0         | Read Only   | boolean  | True if robot is on the arena    |
| optional    | shape       | square    | shapes      | string   | arena display shape              |
| optional    | color       | blue      | colors      | string   | arena display colour             |
| optional    | size        | 250       | [250, 1000] | integer  | arena display size               |
| optional    | alpha       | 1         | Read Only   | float    | arena display transparency       |
| optional    | weight      | 50        | Read Only   | integer  | weight of robot                  |
| optional    | payload     | 100       | Read Only   | integer  | maximum load of robot            |
| optional    | cargo       | None      | object      | object   | object robot is transporting     |
| optional    | station     | None      | object      | object   | station robot is heading for     |
| optional    | distance    | 0         | Read Only   | float    | distance travelled by robot      |
| optional    | energy      | 0         | Read Only   | float    | energy consumed by robot         |
| optional    | kind_class  | Robot     | Read Only   | string   | class of kind                    |

**Droid Class Attributes**

| required    | name        | default   | validation  | datatype | description                      |
| ----------- | ----------- | --------- | ----------- | -------- | -------------------------------- |
| required    | kind        | Droid     | Read Only   | string   | robot class type                 |
| required    | coordinates | [0, 0, 0] | [2, 2, 0]   | list     | x, y, z location of a robot      |
| required    | max         | [2, 2, 0] | Read Only   | list     | maximum x, y, z velocity         |
| required    | velocity    | [0, 0, 0] | [2, 2, 0]   | list     | current x, y, z velocity         |
| recommended | soc         | 2000      | Read Only   | integer  | state of charge of battery       |
| recommended | capacity    | 2000      | Read Only   | integer  | energy capacity of robot battery |
| optional    | shape       | circle    | shapes      | string   | arena display shape              |
| optional    | color       | red       | colors      | string   | arena display colour             |
| optional    | size        | 300       | [250, 1000] | integer  | arena display size               |
| optional    | weight      | 100       | Read Only   | integer  | weight of robot                  |
| optional    | payload     | 200       | Read Only   | integer  | maximum load of robot            |

**Drone Class Attributes**

| required    | name        | default   | validation  | datatype | description                      |
| ----------- | ----------- | --------- | ----------- | -------- | -------------------------------- |
| required    | kind        | Drone     | Read Only   | string   | robot class type                 |
| required    | coordinates | [0, 0, 0] | [3, 3, 1]   | list     | x, y, z location of a robot      |
| required    | max         | [3, 3, 1] | Read Only   | list     | maximum x, y, z velocity         |
| required    | velocity    | [0, 0, 0] | [3, 3, 1]   | list     | current x, y, z velocity         |
| recommended | soc         | 500       | Read Only   | integer  | state of charge of battery       |
| recommended | capacity    | 500       | Read Only   | integer  | energy capacity of robot battery |
| optional    | shape       | triangle  | shapes      | string   | arena display shape              |
| optional    | color       | green     | colors      | string   | arena display colour             |
| optional    | size        | 500       | [250, 1000] | integer  | arena display size               |
| optional    | weight      | 25        | Read Only   | integer  | weight of robot                  |
| optional    | payload     | 50        | Read Only   | integer  | maximum load of robot            |

### **Robot Methods**

---

You need to define methods and supporting attributes to help control and manage robots within your own operational strategy for the ecosystem.

A recommended design strategy is documented below, but alternative designs are welcome.

**Moving**

The key ability of a robot is to move, whilst obeying ecosystem rules is essential. Moving means changing the coordinates `[x, y, z]`. Robots can in theory be made to move my setting their coordinates from outside.

    rob = Robot('robbo')
    rob.coordinates = [21,13,0]
    ecosystem.update

As long as the move did not transgress the ecosystem rules for movement, your robot should be ok. However, if we are to follow the priniples of object oriented design, we should **encapsulate** the move behavour in methods for the base class, Robot.

Two attributes of the classes exist which can be used to design an efficient move method to update coordinates.

    self.max
    self.velocity
    self.coordinates

It is recommended you design a move method, which changes the `coordinates` of a robot. The method should respect the configuration of `velocity` (3 vectors, positive or negetive), the maximum **absolute** values of the permitted velocity given by `max`. Any changes to coordinates, and the system boundary.

**Targetting**
You robot has to learn to respond to an instruction to go to specific location. this is a necessary skill when it is in a fully operating ecosystem.

It is recommended therefore you furnish robots with the `target` attribute (default `None`) which represents a set of $x, y, z$ coordinates for a target designation (it is uncertain whether sky targets will exist in the future).

Your `velocity` attribute can be adjusted before each update i.e. in the update loop, to respond to a robots current `coordinates` and the `target` coordinates, whilst of course respecting the `max` value . This will require some careful thinking.

# **Courseworks Stages**

## **Stage 1 - Robot Classes**

**Tasks**

-   Train yourself on the Ecosystem - see Stage 1 slide deck and the Ecosystem Documentation above.

-   Begin the design your robot classes here. You will develop your classes further to improve their functionality as you move through the stages.
    There are three classes you need initially.

    -   Robot - _base_ or parent class
    -   Droid - inherits from Robot
    -   Drone - inherits from Robot

    Interface requirements and recommendations for attributes (instance variables and methods) are documented above. See the stage 1 slide deck on Learn

    -   Tests - There are three code cells below for running test.

    -   Use

## **Stage 1 Testing**

There are 3 exercises in this section

**Tasks**

1. Test 1 Instantiation
2. Test 2 Attributes Zone
3. Test 3 -Robot Factory

Use the code cells below to test your robot classes. You should ensure:

-   Your robots, droids and drones classes instantiate without any error
-   Instantiate an ecosystem, register robots and and display them.

See the slide deck for stage 1 for more information. Make sure you follow the ecosystem training. Note that automatic code testing will check your robots can perform as suggested in these test cells

## **Stage 2 - Training and Development**

There are seven exercises in this section.

Some are simple adaptations of the previous, so copy your code forwards.
Create a new ecosystem for each exercise so it is independent of previous code cells.

This section principally involves the development of 'methods' for your robot.

Methods should be fully developed for the parent Robot class, and Droids and Drones should **inherit** these methods. However you should ensure that subtle differences in their attribute values should be respected by the method so that it works correctly for each type of Robot.

**Tasks**

1. Creating Pizzas
1. Target and Move
1. Deliveree
1. Work Till You Drop
1. Factoree Deliveree
1. Charger
1. Factory Delivery and Charging

**Please see slide deck on Stage 2 for guidance on these exercises.**

## **Stage 3 - Ecosystem Monitoring**

Stage 3 is about extracting some numbers from your ecosystem of robots

By now with completion of **Factory Delivery and Charging** you should have successful ecosystem runs for several hundred hours, even 1000.
With at least 10 robots delivering and charging. You might be losing some along
the way but don't worry about this.

But how do we know what has quantitatively been done. What data has been created along the way? There are five data points that are collected for each robot, which you will have seen in the attribute table:

| required    | name     | default | validation | datatype | description                     |
| ----------- | -------- | ------- | ---------- | -------- | ------------------------------- |
| recommended | service  | 0       | Read Only  | integer  | service points accrued by robot |
| recommended | age      | 0       | Read Only  | integer  | age of robot in hours           |
| recommended | active   | 0       | Read Only  | integer  | age of robot in hours           |
| optional    | distance | 0       | Read Only  | float    | distance travelled by robot     |
| optional    | energy   | 0       | Read Only  | float    | energy consumed by robot        |

For metrics, make sure that you have these attributes in your parent Robot class. The ecosystem maintains a record in the registry so the data is collected in any case.

Using these values you can derive:

-   $$Average\ \ speed = \frac{distance}{active\ hours} \  (squares.hour^{-1})$$

-   $$Service\ \ rate = \frac{service}{active\ hours} \  (kg.hour^{-1})$$

-   $$Energy\ \ efficiency\ \  = \frac{Energy}{Service} (kWh.kg^{-1} $$

-   $$Average\ \ Power\ \  = \frac{Energy}{active\ hours}\ \ (kWh.hour^{-1} = kW)$$

All the required values are in the registry so lets access that.

For the exercises in this stage take a **copy** of your working **Factory Charging** code cell in stage 2. As with all your code cells this should start with its own ecosystem instantiation.

## **Stage 4 - Performance**

This section is less prescriptive than the previous sections.

It requires you to try and improve the performance as monitored by your work in stage 3

The benchmark is the key performance indicators (KPIs) of an ecosystem with

-   5 robots of each kind
-   An ecosystem duration of 1000 hours
-   A maximum of **2** charging stations

### **KPIs**

-   $$Robot\ performance = \frac{Active\ hours}{Age} \  (ratio)$$

-   $$Service\ \ rate = \frac{Service}{Active\ hours} \  (kg.hour^{-1})$$

-   $$Energy\ \ efficiency\ \  = \frac{Energy}{Service} (kWh.kg^{-1} $$

-   $$Average\ \ Power\ \  = \frac{Energy}{Active\ hours}\ \ (kWh.hour^{-1} = kW)$$

All these KPIs are affected by the operational efficiency of the Robots and this is determined by

-   poor allocation of pizzas to robots - does the nearest Robot get the next pizza!
-   poor charging decisions - often a robot has to head for the charger based on a low SOC when
    1. it may have been able to collect the pizza it was heading for then headed to the charger.
    2. it could have charged as it was passing the charger i.e. before SOC was too low
    3. High risk taking resulting in robot's running out of charge

So there are two things you can do:

1. Smarter allocation of pizzas to robots on a proximity basis.
2. Smarter charging algorithm.

Do not deliver in your submission, additional or new Robot classes.

Any changes you make to your robot classes in stage 1 should not 'break' earlier exercises!

This section has no specific code cells - please organise as you choose.

If you do incremental improvements with different measures, use a new codecell and show how the improvements unfold.

Document your enhancements, and KPI improvements in the final text cell.

# **On submission**

Answer to the following sections and attach it with your solution.

## **KPI Improvement Summary**

Please summarise your KPI improvements here and how you achieved them.

## **Summary and Conclusion**

This coursework should have highlighted the use of OOP in software design.
Summarise how the 4 main concepts of OOP (see WSA010 JNB08 Structured Programming.ipynb) have been employed in the Robot Ecosystem. Your answer should make specific reference to the objects, methods, attributes and any other code you have deployed in the courswork, and discuss OOPs suitability for this sort of application.

(500 words minimum - 1000 words maximum)

# **Marking Schema**

**_Please use your own work - do not plagiarise_**

|                            Section | Deliverable                                 | Marks | Section | Section % |
| ---------------------------------: | ------------------------------------------- | :---: | :-----: | :-------: |
|                      Robot Classes |                                             |       |   12    |    12%    |
|                                    | Robot Class                                 |   4   |         |           |
|                                    | Droid Class                                 |   4   |         |           |
|                                    | Drone Class                                 |   4   |         |           |
|            Stage 1 - Robot Classes |                                             |       |    8    |    8%     |
|                                    | Test 1 Instantiation                        |   2   |         |           |
|                                    | Test 2 Attributes Zone                      |   2   |         |           |
|                                    | Test 3 -Robot Factory                       |   4   |         |           |
| Stage 2 - Training and Development |                                             |       |   32    |    32%    |
|                                    | Creating Pizzas                             |   2   |         |           |
|                                    | Target and Move                             |   4   |         |           |
|                                    | Deliveree                                   |   4   |         |           |
|                                    | Work Till You Drop                          |   4   |         |           |
|                                    | Factoree Deliveree                          |   6   |         |           |
|                                    | Charger                                     |   4   |         |           |
|                                    | Factory Delivery and Charging               |   8   |         |           |
|     Stage 3 - Ecosystem Monitoring |                                             |       |   12    |    12%    |
|                                    | Monitoring of Factory Delivery and Charging |   2   |         |           |
|                                    | Ecosystem Registry                          |   2   |         |           |
|                                    | Tabulation                                  |   8   |         |           |
|              Stage 4 - Performance |                                             |       |   16    |    16%    |
|                                    | KPI Improvements                            |  10   |         |           |
|                                    | KPI Improvement Summary                     |   6   |         |           |
|             Summary and Conclusion |                                             |       |    8    |    8%     |
|                                    | 500-1000 words                              |   8   |         |           |
|             Python Code Evaluation |                                             |       |   12    |    12%    |
|                                    | Structured Code                             |   8   |         |           |
|                                    | Readable code                               |   4   |         |           |
|                                    |                                             |       |         |           |
|                                    | Total                                       |  100  |   100   |   100%    |

**Robot Classes CRP Notes**

\*\*CRPs should augment code cells with text cells to document designs for third parties and assessment in your final submission. It's good practice to developing working text cells for this purpose from day 1
