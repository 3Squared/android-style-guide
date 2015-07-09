# Android Style Guidelines

* Use Android Studio for all development.
* All dependencies should be specified in the Gradle file. Only add a JAR library if a dependency is not available through Gradle and a suitable alternative cannot be found.
* The Android Support Libraries should be used to ensure compatibility with older devices.
* All layout should be done using Android XML Layout files.
	* All layout widget IDs should be prepended with the widget type e.g. a TextView displaying a title would be 'textView_title'.
* Use the provided formatting rules in Android Studio to ensure code is formatted consistently (CTRL+ALT+L in a file to format it).
	* Import the formatting rules (settings.jar) by going to File > Import Settings... in Android Studio.
* For naming conventions, please refer to [http://google.github.io/styleguide/javaguide.html](http://google.github.io/styleguide/javaguide.html).
