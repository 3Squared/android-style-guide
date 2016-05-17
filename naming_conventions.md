XML Naming Conventions
======================

Basic Principle
---------------
All resource names are constructed from various components.

#### `<WHAT>`
Indicate what the resource actually represents. This is often a standard Android view class. There are
limited options per resource type. See the examples below for more detail.

#### `<WHERE>`
Describe where the resource logically belongs in the app. Resources used in multiple screens use `all`, 
while others use the custom part of the Android view subclass they are in.

#### `<DESCRIPTION>`
Differentiate multiple elements in one screen.

#### `<SIZE>`
Identify the size of the element used one of several metrics.

Advantages
----------

1.  **Ordering of resources by screen**    
	The `<WHERE>` part describes what screen a resource belongs to. Hence it is easy to get all IDs, drawables, 
	dimensions, etc. for a particular screen.

2.  **Strongly typed resource IDs**    
	For resource IDs, the `<WHAT>` describes the class name of the xml element it belongs to. This makes it
	easy to identify what to cast your `findViewById()` calls to.

3.  **Better resource organizing**    
	File browsers/project navigator usually sort files alphabetically. This means layouts and drawables 
	are grouped by their `<WHAT>` (activity, fragment, etc.) and `<WHERE>` prefix respectively. A simple Android 
	Studio plugin/feature could even display these resources as if they were in their own folder.

4.  **More efficient autocomplete**    
	Because resource names are far more predictable, using the IDE's autocomplete becomes even easier. 
	Usually entering the `<WHAT>` or `<WHERE>` is sufficient to narrow autocomplete down to a limited set of 
	options.

5.  **No more name conflicts**    
	Similar resources in different screens are either `all` or have a different `<WHERE>`. A fixed naming 
	scheme avoids all naming collisions.

6.  **Cleaner resource names**    
	All resources will have a more logical naming scheme, causing a cleaner Android project overall.

7.  **Tools support**    
	This naming scheme could easily be supported by Android Studio offering features such as: lint rules to 
	enforce these names, refactoring support when you change a `<WHAT>` or `<WHERE>` and better resource 
	visualisation in project view.


Layouts
-------
Layout resources are relatively simple, as there are usually only a few files per screen. Therefore the 
rule can be simplified to:

### `<WHAT>_<WHERE>.xml`

`<WHAT>` is one of the following:

|  Prefix    | Usage
|  ----------| ---------------------------------------
|  activity  | contentview for activity
|  fragment  | view for a fragment
|  view      | inflated by a custom view
|  item      | layout used in list/recycler/gridview
|  layout    | layout reused using the include tag

Examples:

-  **activity\_main**: content view of the MainActivity
-  **fragment\_articledetail**: view for the ArticleDetailFragment
-  **view\_menu**: layout inflated by custom view class MenuView
-  **item\_article**: list item in ArticleRecyclerView
-  **layout\_footer**: layout for a footer included in several layouts throughout the app


Strings
-------
Strings are accessed using `R.string`, so `<WHAT>` is irrelevant. Either we use `<WHERE>` to indicate 
where the string will be used:

### `<WHERE>_<DESCRIPTION>`

or `all` if the string is reused throughout the app:

### `all_<DESCRIPTION>`

Examples:

-  **articledetail\_title**: title of ArticleDetailFragment
-  **feedback\_explanation**: explanation in FeedbackFragment
-  **all\_done**: generic "done" string

`<DESCRIPTION>` can be split into multiple parts for improved heirarchy. For example, in a form capturing
information about an employee, you could have: 

-  **employeeform\_personal\_name**: name of individual 
-  **employeeform\_personal\_address**: address of individual
-  **employeeform\_company\_name**: name of company

In the example above, the `personal` and `company` parts of the ID pertain to different sections of 
the `employeeform` layout.

Drawables
---------
For drawables, we use either `<WHERE>` to indicate where the drawable will be used:

### `<WHERE>_<DESCRIPTION>`

or `all` if the drawable is reused throughout the app:

### `all_<DESCRIPTION>`

`<WHAT>` can be prepended to `<DESCRIPTION>` in cases where the drawable has firm type. For example: icons,
backgrounds, selectors and dividers.

### `<WHERE>_<WHAT>_<DESCRIPTION>`

For drawables such as icons, a `<SIZE>` argument can be added. This can be an actual size (e.g. "24dp"), a
percentage (e.g. 100) or a size qualifier (e.g. "small").

### `<WHERE>_<DESCRIPTION>_<SIZE>`

Examples:

-  **articledetail\_placeholder**: placeholder in ArticleDetailFragment
-  **all\_icon\_info**: generic info icon
-  **all\_icon\_info\_24dp**: 24dp version of generic info icon
-  **all\_divider\_vertical\_100**: a vertical divider to be used in LinearLayout at 100% of the app's 
                                    default spacing


IDs
---
For IDs, `<WHAT>` is the class name of the xml element it belongs to. `<DESCRIPTION>` is used to distinguish 
similar elements in one screen. `<DESCRIPTION>` can be split into multiple parts for improved hierarchy.

### `<WHAT>_<WHERE>_<DESCRIPTION>`

Examples:

-  **toolbar_employeelist**: Toolbar in EmployeeListActivity
-  **imageview\_employeeitem\_profile**: Profile image in list item for employee
-  **textview\_articledetail\_title**: title TextView in ArticleDetailFragment


Dimensions
----------
Apps should only define a limited set of dimensions, which are constantly reused. This makes most 
dimensions `all` by default.

Therefore you should mostly use:

#`<WHAT>_all_<DESCRIPTION>_<SIZE>`

and optionally use the screen specific variant:

#`<WHAT>_<WHERE>_<DESCRIPTION>_<SIZE>`

Where `<WHAT>` is one of the following:

|  Prefix     | Usage
|  -----------| ------------------------------------------------
|  width      | width in dp
|  height     | height in dp
|  size       | if width == height
|  margin     | margin in dp
|  padding    | padding in dp
|  spacing    | generic dimen in dp used in both margin and padding
|  elevation  | elevation in dp
|  keyline    | absolute keyline measured from view edge in dp
|  textsize   | size of text in sp

Note that this list only contains the most used `<WHAT>`s. Other dimensions qualifiers like: rotation, 
scale, etc. are usually only used in drawables and as such less reused.

A `<SIZE>` argument can optionally be added. This can be an actual size (e.g. "24dp") or a percentage 
(e.g. 100).

Examples:

-  **height\_all\_toolbar**: height of all toolbars
-  **keyline\_listtext**: listitem text is aligned at this keyline
-  **textsize\_medium**: medium size of all text
-  **size\_menu\_icon**: size of icons in menu
-  **spacing\_all_100**: the default spacing at 100% used throughout the app
-  **height\_menu\_profileimage**: height of profile image in menu
