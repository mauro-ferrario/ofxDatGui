##ofxDatGui

**ofxDatGui** is a **simple to use**, lightweight, high-resolution graphical user interface for [openFrameworks](http://openframeworks.cc/) inspired by the popular JavaScript  [datgui](http://workshop.chromeexperiments.com/examples/gui/) interface.  

![ofxDatGui](./img/ofxdatgui_02.png?raw=true)


##Features

**ofxDatGui** offers the following features & components:

* Click & Toggle (On/Off) Buttons
* Text Input Fields
* Color Pickers
* Range Sliders
* Dropdown Menus
* 2D Coordinate Pads
* Folders to group components together
* An optional header & footer that allow you to collapse and drag the Gui around

##Installation

**ofxDatGui** is built on top of C++11 and currently requires openFrameworks 0.9.0 or later which you can easily install by [cloning or downloading the repository from Github.](https://github.com/openframeworks/openFrameworks) 

* Once you've downloaded openFrameworks, clone or download this repository and unpack it into your openFrameworks/addons directory.

* Create a new project using the project generator and include **ofxDatGui** by selecting the ```addons``` button in the generator.

* Copy the ```ofxdatgui_assets``` directory in the root of this repository to your newly created project's bin/data directory.

* Add **ofxDatGui** to your project by adding  ```#include "ofxDatGui.h"``` to the top of your ```ofApp.h``` file and you're ready to go!

##Getting Started

To create an **ofxDatGui** simply pass in the X and Y coordinates where you would like it to live or use one of the convenient pre-defined anchors.

	ofxDatGui* gui = new ofxDatGui( 100, 100 );
	ofxDatGui* gui = new ofxDatGui( ofxDatGuiAnchor::TOP_LEFT );
	ofxDatGui* gui = new ofxDatGui( ofxDatGuiAnchor::TOP_RIGHT );

Adding components to **ofxDatGui** is as simple as:

	gui->addButton("Click!");
	
This generates a Basic Button with the label "Click!"

![ofxDatGui](./img/ofxdatgui_click.png?raw=true)

## Components
 
**ofxDatGui** currently offers the following components:
  
**Text Input**
 
	gui->addTextInput(string label, string value = "");
	
![ofxDatGui](./img/ofxdatgui_input.png?raw=true)
  
**Basic Button**
	 	
	gui->addButton(string label);
	
![ofxDatGui](./img/ofxdatgui_click.png?raw=true)	
	
**Toggle Button**

	gui->addToggle(string label, bool enabled = false);


![ofxDatGui](./img/ofxdatgui_toggle.png?raw=true)

**Range Slider**

	gui->addSlider(string label, float min, float max);

![ofxDatGui](./img/ofxdatgui_slider.png?raw=true)
	
Optionally you can set the starting value of the slider.  
If this is not set it will default to halfway between the min and max values.

	gui->addSlider(string label, float min, float max, float value);
	
**Color Picker**	
	
	gui->addColorPicker(string label, ofColor color = ofColor::black);

![ofxDatGui](./img/ofxdatgui_color.png?raw=true)
	
**Dropdown Menu**
	
	vector<string> options = {"ONE", "TWO", "THREE", "FOUR"};
	gui->addDropdown(options);
	
![ofxDatGui](./img/ofxdatgui_dropdown.png?raw=true)

**2D Coordinate Pad**

	gui->add2dPad(string label, ofRectangle bounds);

![ofxDatGui](./img/ofxdatgui_2dpad.png?raw=true)

The bounds parameter is optional and will default to the window dimensions if omitted.

##Component Groups (Folders)

You can also group related components into folders. When constructing a folder pass in a label to name the folder and an optional color to help visually group its contents.

	ofxDatGuiFolder* folder = gui->addFolder("My White Folder", ofColor::white);
	folder->addTextInput("** Input", "A Nested Text Input");
	folder->addSlider("** Slider", 0, 100);
	folder->addToggle("** Toggle", false);

![ofxDatGui](./img/ofxdatgui_folder.png?raw=true)
	
## Manipulation

Every ``gui->add*`` method returns a pointer to an **ofxDatGuiItem** object that you can store in a variable for later manipulation.

	ofxDatGuiTextInput* myInput;
	myInput = gui->addTextInput("Name:", "Stephen");
	myInput->setText("Freddy");
	
Every **ofxDatGuiItem** has a mutable label that can easily be retrieved or changed via:

	item->getLabel();
	item->setLabel(string label);
	
In addition some components have methods (typically getters & setters) that allow for the retrieval and manipulation of its unique properties.
	
**Text Input**
 
	ofxDatGuiTextInput* myInput;
	myInput->getText();
	myInput->setText(string text);
	
**Toggle Button**

	ofxDatGuiToggle* myToggle;
	myToggle->toggle();
	myToggle->setEnabled(bool enable);
	myToggle->getEnabled(); // returns true or false

**Range Slider**

	ofxDatGuiSlider* mySlider;
	mySlider->getScale(); 
	mySlider->setScale(float scale); // a value between 0 & 1 //
	mySlider->getValue();
	mySlider->setValue(float value); // a value between min & max //

**Color Picker**
	
	ofxDatGuiColorPicker* myColorPicker;
	myColorPicker->getColor();
	myColorPicker->setColor(int hexValue);
	myColorPicker->setColor(int r, int g, int b);
	myColorPicker->setColor(ofColor color);
	
**Dropdown Menu**
	
	ofxDatGuiDropdown* myDropdown;
	myDropdown->select(childIndex);
	myDropdown->getSelectedChildIndex(); 
	
**Note:** All indicies are zero based so the first item in your  **ofxDatGui** instance will have an index of 0, the second item will have an index of 1, the third item an index of 2 etc..
	
## Events

**ofxDatGuiEvents** are designed to be as easy as possible to interact with.

To listen for an event simply register a callback to be executed when an event you care about is fired:
	
	gui->onButtonEvent(this, &ofApp::onButtonEvent);
	void onButtonEvent(ofxDatGuiButtonEvent e)
	{
		cout << "A button was clicked!" << endl;
	}
    
Every callback you register will receive an event object that contains a pointer (called target) to the object that created the event. 

	gui->addButton("My Button");
	gui->onButtonEvent(this, &ofApp::onButtonEvent);
	void onButtonEvent(ofxDatGuiButtonEvent e)
	{
		cout << e.target->getLabel() << endl; // prints "My Button"
	}

If you saved the pointer returned by ``gui->add*`` in a variable you can compare it to the event target to decide how to handle the event. 

	ofxDatGuiButton* b1 = gui->addButton("Button 1");
	ofxDatGuiButton* b2 = gui->addButton("Button 2");
	gui->onButtonEvent(this, &ofApp::onButtonEvent);
	void onButtonEvent(ofxDatGuiButtonEvent e)
	{
		if (e.target == b1){
			cout << "Button 1 was clicked" << endl;
		} else if (e.target == b2){
		// 	button 2 was clicked, do something else //
		}
	}

All events also contain additonal properties that allow convenient access to the state of the component that dispatched the event.

**ofxDatGuiButtonEvent**

	ofxDatGuiButtonEvent e
	bool e.enabled // the enabled state of a toggle button
			
**ofxDatGuiSliderEvent**

	ofxDatGuiSliderEvent e
	float e.value // current value of the slider 
	float e.scale // current scale of the slider 

**ofxDatGuiTextInputEvent**
	
	ofxDatGuiTextInputEvent e
	string e.text // current text in the textfield
	
**ofxDatGuiColorPickerEvent**

	ofxDatGuiColorPickerEvent e
	ofColor e.color // the color of the picker
	
**ofxDatGui2dPadEvent**

	ofxDatGui2dPadEvent e
	float e.x // x coordinate within the pad's bounds rectangle
	float e.y // y coordinate within the pad's bounds rectangle
	 
**ofxDatGuiDropdownEvent**
	
	ofxDatGuiDropdownEvent e
	int e.child // the index of the selected option (zero based)
	
**Note:** You can always retrieve these properties directly from the event target itself.

	ofxDatGuiSliderEvent e
	float value = e.target->getValue();
	float scale = e.target->getScale();
	
		
##Headers & Footers

**ofxDatGui** also provides an optional header and footer that allow you to title your gui, drag it around and conveniently collapse and expand it.

	gui->addHeader(":: Drag Me To Reposition ::");
	
![ofxDatGui](./img/ofxdatgui_header.png?raw=true)
	
	gui->addFooter();
 
![ofxDatGui](./img/ofxdatgui_footer.png?raw=true)
 
##Customization 

Eventually custom themes will be supported however for now you can adjust the transparency of the gui via: 
 
	gui->setOpacity(float opacity); // between 0 & 1 // 
 
You can also show & hide **ofxDatGui** by pressing the ``h`` key.

##Additonal Notes

Thanks for reading all this and checking the project out. 

I'm actively looking for people to help me test this and provide feedback to help shape ongoing development. If you'd like to see a feature prioritized or have any general questions or feedback please send me a message or open an issue here on Github.

Thanks!