# LVGL for MicroBlocks

## Table of Content

[Introduction](#introduction)
- [Integration in MicroBlocks](#integration-in-microblocks)
- [Firmware](#firmware)
- [Library](#library)
- [Examples](#examples)
  
[LVGL library](#lvgl-library)
- [Objects](#objects)
- [General modifiers](#general-modifiers)
- [Button](#button)
- [Label](#label)
- [Arc](#arc)
- [Slider](#slider)
- [Bar](#bar)
- [Led](#led)
- [Switch](#switch)
- [Roller](#roller)
  
[More complicated widgets](#more-complicated-widgets)
- [List](#list)
- [Tabview](#tabview)
- [Tileview](#tileview)

[Events](#events)
- [Manual event handling](#manual-event-handling)
- [Integrated event handling](#integrated-event-handling)


# Introduction

LVGL (Light and Versatile Graphics Library) is a GUI library for embedded systems with a graphical display and some form of input (such as a touchscreen, rotary encoder, etc.). It helps create attractive user interfaces, with the logic and rendering handled entirely by the LVGL library. The user only needs to focus on the layout and responds to events triggered by user interactions.

## Integration in MicroBlocks

LVGL is a complex library that offers a wide variety of widgets. Each widget supports numerous style settings and configuration options. One of the strengths of MicroBlocks is its self-explanatory nature, where the blocks clearly convey their function. We have aimed to stay as close to that principle as possible in this integration.

The LVGL version of MicrobBlocks can be found on [this github](https://github.com/ste7anste7an/smallvm_lms). For fast screen processing, we needed a DMA capable graphics library. Unfortunaltely, the Adafruit GFX library does not support DMA. The graphics has been converted to [TFT_eSPI](https://github.com/Bodmer/TFT_eSPI/) that uses double buffering with DMA. Furthermore, PSRAM is used for storing large display buffers.

The LVGL MicroBlocks version still supports all the functions from the original version.

## Firmware
Currently we have a pre-compiled firmware for the M5Stack Core 2 platform. The [firmware can be downloaded](https://github.com/ste7anste7an/smallvm_lms/raw/refs/heads/dev/lvgl_mb/firmware/firmware_MicroBlocks_LVGL_m5core2-lvgl_20250615-154059.bin) from the projects github and once downloaded, it can be dragged in the IDE when an M5Stack Core2 is connected via USB.

## Library
You need to add the LVGL library to Microblocks. Download the `lvgl.ubl` [library from the github](https://github.com/ste7anste7an/smallvm_lms/raw/refs/heads/dev/lvgl_mb/microblocks/lvgl.ubl) and drag in into the library area in the IDE.

## Examples

Some example code that can be run on an M5Stack Core 2 without additional hardware, can be found in the [M5core 2 LVGL example directory](https://github.com/ste7anste7an/smallvm_lms/tree/dev/lvgl_mb/microblocks/m5core2).

# LVGL library
## Objects
In MicroBlcoks all widgets objects have a name. The name is used to reference its objects. If you create a button with name `btn`, than you can use the name `btn` to set its position, change it's size and change it's text. The objects are internally stored in a map in which the objects are indexed with their names. 

## General modifiers

A number of general modifiers are available for most of the widgets

![set_pos](https://github.com/user-attachments/assets/b7b28433-2294-4a2c-943a-eab7746bbcbf)

position: determines the position relative to its parent

![set_size](https://github.com/user-attachments/assets/74c7d23d-6fa6-489b-b6ed-1b0109267687)
  
size: changes the size of a widget.

![set_color](https://github.com/user-attachments/assets/42165ebc-e490-4853-8675-ec7e06d5cc47)

color: changes the colors of attributes of a widget. The first color is usually the background color, the second color is the color for the indicator (e.g. for an Arc, a bar or a slider) and the third color is the color for the knob (for Arc and Slider).
  

## Button
![image](https://github.com/user-attachments/assets/cf3ad661-0cdf-4369-82bb-1181d9521c66)

A button is created by the `btn` block.

![simple_button](https://github.com/user-attachments/assets/df9415f9-dd93-417a-8d19-aa708c69fa11)

Here, for simplicity, the name of the button is automatically used as the text written in the button. So if you create a button with name `OK`, a button with `OK` written in it is created.

![expanded_button](https://github.com/user-attachments/assets/835d8e87-87c5-4a9f-bbc7-c2b186893191)

The button blocks have a number of extra fields. You can change the text size ranging from 1 to 4 mapping on a 14, 24, 40 and 48 points font. A separate `text` field allows for a text that is different than the button name. The `parent` field refers to the name of a parent object. The default value is the main screen (`lv_scr_act`). A parent can be a tab in a tabview or a tile in a tileview.

## Label
![image](https://github.com/user-attachments/assets/b98ddd9a-365a-4311-9191-1a698a0d3c87)

A label is created using the `label` block.

![simple_label](https://github.com/user-attachments/assets/3e91b30f-00a6-4c0c-b7f4-49c8d4fa3a01)

Like the button block, here the text in the label defaults to the name of the label.

![expanded_label](https://github.com/user-attachments/assets/efa0c3f1-d7f4-4ab7-a02b-0f302039e06e)

The expanded label block has similar fields as the button block.

## Arc
![image](https://github.com/user-attachments/assets/8ba122d0-5431-4e28-bedb-abff209f7a3b)

An arc is drawn using the generic add object block, where from the drop down `arc` is selected.

![add_arc](https://github.com/user-attachments/assets/73293166-0a86-4b06-921a-2e29d242f33c)

Some of the properties of the arc can be changed.

![set_range](https://github.com/user-attachments/assets/2c3e8244-5e5c-47ee-81c4-7e4649798671)

Changes the start and end scale of the arc (default 0 to 100)

![set_angles](https://github.com/user-attachments/assets/244bce33-abd0-4adf-9394-fdaacf4cbc63)

Changes the start and end angle of the scale of an arc. Zero degrees is at the middle right (3 o'clock) of the object and the degrees increase in clockwise direction. The angles should be in the [0;360] range.

![set_rotation](https://github.com/user-attachments/assets/71021e68-68cb-42f4-a173-6dd0515c5d11)

Determines the angle of the 0 degree point on the scale with respect to the background.

![get_val](https://github.com/user-attachments/assets/a15f9069-f6d2-4356-8c52-892fc4ea70cb)

Returns the value of the indicator.

![set_val](https://github.com/user-attachments/assets/6b2cceef-33d4-4378-8ea6-73658031a21c)

Detemines the position of the indicator.

## Slider
![image](https://github.com/user-attachments/assets/f22fa70d-76bb-4393-a963-9ef8e5e4326b)

A slider is drawn using the generic add object block, where from the drop down `slider` is selected.

![add_slider](https://github.com/user-attachments/assets/9020a5e0-fbf9-43de-8a7e-7d74cc308aa0)

Some of the properties of a slider can be changed.

![set_range](https://github.com/user-attachments/assets/2c3e8244-5e5c-47ee-81c4-7e4649798671)

Changes the start and end scale of the arc (default 0 to 100)

![get_val](https://github.com/user-attachments/assets/a15f9069-f6d2-4356-8c52-892fc4ea70cb)

Returns the value of the indicator.

![set_val](https://github.com/user-attachments/assets/6b2cceef-33d4-4378-8ea6-73658031a21c)

Detemines the position of the indicator.

## Bar
![image](https://github.com/user-attachments/assets/12bfc7c6-257c-4b7d-8e1a-c64a6a24c759)

A bar is drawn using the generic add object block, where from the drop down `bar` is selected.

## Led
![image](https://github.com/user-attachments/assets/71031ef4-ddac-4011-b372-3426495f442a)

The value of an led (whether it is on or off) can be set by the `set_val` block with a boolena as argument:

![set_val](https://github.com/user-attachments/assets/467a0674-3209-42e0-96c2-d83671495cd3)

The brightness of an led widget is determined by using the properties block:

![set_brightness](https://github.com/user-attachments/assets/825aea54-84b7-4e6d-af1c-e06dc652f5a6)

## Switch
![image](https://github.com/user-attachments/assets/c7d51e6b-61fc-4c00-9d15-f3297859baf6)

A switch is drawn using the generic add object block, where from the drop down `switch` is selected.

When a switch is toggled in the GUI, an event is generated and the value of the switch can be read using:

![get_val](https://github.com/user-attachments/assets/1c946273-e200-461f-a84f-5f5ec4ed086d)

Sometimes, you want the switch in an initial position. for this you use the `set_val` block 

![set_val](https://github.com/user-attachments/assets/335d00e4-2c0d-4ac1-bc0d-e0dfed6afeae)

Where the parameter is a boolean.


## Roller
![image](https://github.com/user-attachments/assets/aa05cb08-553a-4405-8b59-302ebd7c505d)

A roller is drawn using the generic add object block, where from the drop down `roller` is selected.

![add_roller](https://github.com/user-attachments/assets/d5562999-eaa2-4977-8786-fb7ac6391438)

Once a roller is added, it will show some default options. These can be overridden by using the `set_text` block

![roller_set_text](https://github.com/user-attachments/assets/31185a84-6669-4012-b111-8701d7ded333)

When a value is selected in a roller, an event is created and the string of the selected line is returned with the `get_event`.

![get_val](https://github.com/user-attachments/assets/d4b4c264-e284-485a-bca0-791eb875abb5)



# More complicated widgets

The following widgets are more complex in nature and make use of the `set [] parent [ ]` block, which allows to determine the hierarchy of one widget to another. The container widgets `list`, `tabview` and `tileview` are widgets that can act as a parent for widgets like `buttons` or `labels', etc.

## List
![image](https://github.com/user-attachments/assets/81164ee9-2119-41b7-9d29-416cbba2b957)

A list is drawn using the generic add object block, where from the drop down `list` is selected.

![add_list](https://github.com/user-attachments/assets/0d454826-9c53-4a3f-97ac-65cb5d062eee)

When a list is added, you will see an empty area. Now you can add other widgets to the list by setting their parent field to the list name.

![add_button_to_list](https://github.com/user-attachments/assets/a256ffd7-816e-4f94-8206-82d05b03cfbb)

Another way of adding widgets to a list, is by using the `set_parent` block on an existing widget to add it to a list.

## Tabview
The Tab view object can be used to organize content in tabs. Look in the lVGL documentation for an [example of a tabview](https://docs.lvgl.io/9.2/widgets/tabview.html#simple-tabview).

You start by creating a tabview.

![add_tabview](https://github.com/user-attachments/assets/cdcb7447-443e-4385-8023-03aa42499e82)

You do no see anything happening until you ad a new tab using

![add_tab](https://github.com/user-attachments/assets/974232de-90d4-4857-9ef8-3bd775db1b6d)

Where the parent refers to the tabview's name.

Once a tab is created, any widget can be added to the tab by setting the `parent` to the specific tab's name.

## Tileview
The Tile view is a container object whose elements (called tiles) can be arranged in grid form. A user can navigate between the tiles by swiping. Any direction of swiping can be disabled on the tiles individually to not allow moving from one tile to another.

If the Tile view is screen sized, the user interface resembles what you may have seen on smartwatches. Look in the LVGL documentation for an [example of a tileview](https://docs.lvgl.io/9.2/widgets/tileview.html#tileview-with-content)

You start by creating a tileview.

![add_tileview](https://github.com/user-attachments/assets/ca574587-1030-46bb-b408-a0ce31e0392a)

Within this tileview new tiles can be created using

![add_tile](https://github.com/user-attachments/assets/5981c89f-da71-468f-8db0-73a00c883ac5)

As a parent you take the `tileview` name that was created before. A new tile is created on column `col` and row `row`. The booleans define whether you can swipe in the directory (left, right, top, bottom) to reach a next tile.

Once a tile is created, any widget can be added to the tile by setting the `parent` to the specific tile's name.

# Events

## Manual event handling
The following widgets generate an event when their value changes: button, arc, slider, switch, and roller. When an event occurs, the message `LVGLevent` is broadcasted. So the first way to handle such an event is to wait for this broadcast:

![when_lvglevent](https://github.com/user-attachments/assets/f6e4f228-a6ca-4533-b268-bdff8ba824fd)

There is also a boolean function `event_available` that can be used in a `when` block:

![when_lvglevent](https://github.com/user-attachments/assets/f6e4f228-a6ca-4533-b268-bdff8ba824fd)

When the evwnt is caught, the function

![get_event](https://github.com/user-attachments/assets/7434fba7-9afe-4d46-aee7-d93dcd0f5cf6)

returns the name of the widget that caused the event. Within the `When` clause, we can check which widget caused the event and handle appropriately by reading the value of the widget.

The following event handler checks whether a button `button` is pressed . It calls a function to handle it accordingly, or it checks whether an `arc` widget changed its value and stores the value of the `arc` in a global variable.

![handle_event-button_arc](https://github.com/user-attachments/assets/795acefd-a91f-4c16-8411-c9666241ed87)

## Integrated event handling

We provide three helper function blocks for handling events in a more integrated way.

![init_callback](https://github.com/user-attachments/assets/d947283c-d000-4a1a-a077-b12fbb66a4f2)

Initializes the callback handling. This should be called once at the beginning of the program.

![add-callback](https://github.com/user-attachments/assets/59c0c7bf-2a9c-44ec-b4cd-d90c084bba36)

This block adds an event callback function to a widget's name. The function must be defined as a `command block` and it's name is eneterd as a string. Every widget that creates an event should have it's own callback function.

![handle_event](https://github.com/user-attachments/assets/80e111e6-5787-4d54-8a01-51341a47ced1)

The eventhandler itself is called in the `When ` block that checks for new events. As an argument the `get_event` is passed.

Below is an example program that shows how to handle different events with each of their own callback function.

![example_event_handler](https://github.com/user-attachments/assets/8391e205-cda1-45d4-8e07-5dd077f78bca)


