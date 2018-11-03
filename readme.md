## ofxDatGui

Now it's possibile to load and save data from json using [ofxJsonSettings custom fork](https://github.com/mauro-ferrario/ofxJsonSettings).

// Load settings

gui->loadSettings("data.json");

or, when you have multiple gui you can load the "data.json" the first time and after use the loaded data for all the other gui:

gui1->loadSettings("data.json"); // First load the "global" data for all the gui and use for the gui1

gui2->loadSettings(); // than use the data just loaded also for gui2


// Save settings

gui->saveSettings("data.json");

or

gui->saveSettings(); // Save sata using the name of the last loaded json