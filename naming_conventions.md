XML naming conventions
======================

Basic principle
---------------

All resource names follow a simple convention.

#`<WHAT>_<WHERE>_<DESCRIPTION>_<SIZE>`

Let's first describe every element briefly. After the advantages, we'll
see how this applies to each resource type.

####`<WHAT>`

Indicate what the resource actually represents, often a standard Android
view class. Limited options per resource type. (e.g. MainActivity -&gt;
`activity`)

#### `<WHERE>`

Describe where it logically belongs in the app. Resources used in
multiple screens use `all`, all others use the custom part of the
Android view subclass they are in. (e.g. MainActivity -&gt; `main`,
ArticleDetailFragment -&gt; `articledetail`)

#### `<DESCRIPTION>`

Differentiate multiple elements in one screen. (e.g. `title`)

#### `<SIZE>` (optional)


Advantages
----------

1.  **Ordering of resources by screen**
     The `WHERE` part describes what screen a resource belongs to. Hence
    it is easy to get all IDs, drawables, dimensions,... for a
    particular screen.
2.  **Strongly typed resource IDs**
     For resource IDs, the `WHAT` describes the class name of the xml
    element it belongs to. This makes is easy to what to cast your
    `findViewById()` calls to.
3.  **Better resource organizing**
     File browsers/project navigator usually sort files alphabetically.
    This means layouts and drawables are grouped by their `WHAT`
    (activity, fragment,..) and `WHERE` prefix respectively. A simple
    Android Studio plugin/feature can now display these resources as if
    they were in their own folder.
4.  **More efficient autocomplete**
     Because resource names are far more predictable, using the IDE's
    autocomplete becomes even easier. Usually entering the `WHAT` or
    `WHERE` is sufficient to narrow autocomplete down to a limited set
    of options.
5.  **No more name conflicts**
     Similar resources in different screens are either `all` or have a
    different `WHERE`. A fixed naming scheme avoids all
    naming collisions.
6.  **Cleaner resource names**
     Overall all resources will be named more logical, causing a cleaner
    Android project.
7.  **Tools support**
     This naming scheme could be easily supported by the Android Studio
    offering features such as: lint rules to enforce these names,
    refactoring support when you change a `WHAT` or `WHERE`, better
    resource visualisation in project view.


Layouts
-------

Layouts are relatively simple, as there usually are only a few layouts
per screen. Therefore the rule can be simplified to:

#`<WHAT>_<WHERE>_.XML`

Where `<WHAT>` is one of the following:

|  Prefix    | Usage
|  ----------| ---------------------------------------
|  activity  | contentview for activity
|  fragment  | view for a fragment
|  view      | inflated by a custom view
|  item      | layout used in list/recycler/gridview
|  layout    | layout reused using the include tag

Examples:

-   **activity\_main**: content view of the MainActivity
-   **fragment\_articledetail**: view for the ArticleDetailFragment
-   **view\_menu**: layout inflated by custom view class MenuView
-   **item\_article**: list item in ArticleRecyclerView
-   **layout\_actionbar\_backbutton**: layout for an actionbar with a
    backbutton (too simple to be a customview)


Strings
-------

The `<WHAT>` part for Strings is irrelevant. So either we use `<WHERE>`
to indicate where the string will be used:

#`<WHERE>_<DESCRIPTION>`

or `all` if the string is reused throughout the app:

#`all_<DESCRIPTION>`

Examples:

-   **articledetail\_title**: title of ArticleDetailFragment
-   **feedback\_explanation**: feedback explanation in FeedbackFragment
-   **feedback\_namehint**: hint of name field in FeedbackFragment
-   **all\_done**: generic "done" string

`<WHERE>` obviously is the same for all resources in the same view.


Drawables
---------

The `<WHAT>` part for Drawables is irrelevant. So either we use
`<WHERE>` to indicate where the drawable will be used:

#`<WHERE>_<DESCRIPTION>_<SIZE>`

or `all` if the drawable is reused throughout the app:

#`all_<DESCRIPTION>_<SIZE>`

Optionally you can add a `<SIZE>` argument, which can be an actual size
"24dp" or a size qualifier "small".

Examples:

-   **articledetail\_placeholder**: placeholder in ArticleDetailFragment
-   **all\_infoicon**: generic info icon
-   **all\_infoicon\_large**: large version of generic info icon
-   **all\_infoicon\_24dp**: 24dp version of generic info icon


IDs
---

For IDs, `<WHAT>` is the class name of the xml element it belongs to.
Next is the screen the ID is in, followed by an optional description to
distinguish similar elements in one screen.

#`<WHAT>_<WHERE>_<DESCRIPTION>`

Examples:

-   **tablayout\_main** -&gt; TabLayout in MainActivity
-   **imageview\_menu\_profile** -&gt; profile image in custom MenuView
-   **textview\_articledetail\_title** -&gt; title TextView in
    ArticleDetailFragment


Dimensions
----------

Apps should only define a limited set of dimensions, which are
constantly reused. This makes most dimensions `all` by default.

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
|  elevation  | elevation in dp
|  keyline    | absolute keyline measured from view edge in dp
|  textsize   | size of text in sp

Note that this list only contains the most used `<WHAT>`s. Other
dimensions qualifiers like: rotation, scale,... are usually only used in
drawables and as such less reused.

Examples:

-   **height\_toolbar**: height of all toolbars
-   **keyline\_listtext**: listitem text is aligned at this keyline
-   **textsize\_medium**: medium size of all text
-   **size\_menu\_icon**: size of icons in menu
-   **height\_menu\_profileimage**: height of profile image in menu


Known limitations
-----------------

1.  **Screens need to have unique names**
     To avoid collisions in the `<WHERE>` argument, View (like) classes
    must have unique names. Therefore you cannot have a "MainActivity"
    and a "MainFragment", because the "Main" prefix would no longer
    uniquely identify one `<WHERE>`.

2.  **Refactoring not supported**
     Changing class names does not change along resource names
    when refactoring. So if you rename "MainActivity" to
    "ContentActivity", the layout "activity\_main" won't be renamed
    to "activity\_content". Hopefully Android Studio will one day add
    support for this.

3.  **Not all resource types supported**
     The proposed scheme currently does not yet support all
    resource types. For some resources this is because they are less
    frequently used and tend to be very varied (e.g. raw and assets).
    For other resources this is because they are a lot harder to
    generalize (e.g. themes/styles/colors/animations).

    Your apps colors palette likely wants to reuse the terminology of
    your design philosophy. Animations can range from modest (fade) to
    very exotic. Themes and styles already have a naming scheme that
    allows you to implicitly inherit properties.
