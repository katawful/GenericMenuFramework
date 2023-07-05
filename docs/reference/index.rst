GenericMenuFramework Reference
==============================

GenericMenuFramework is designed to be easily accessible by the author. A wide breadth of
user-defined functions are provided. In order to achieve this, this plugin has defined a clear
naming scheme to these functions:

::

   PrefixVerbAction

For example ``GMFGetTileName`` clearly defines each aspect. ``GMF`` is a prefix for this plugin (a
public function), ``Get`` is the verb this function is doing (in this case getting a value), and
``TileName`` is the action this verb is operating upon (the name of a menu tile). Putting this
together, this function is clearly a public function from GenericMenuFramework that gets a tile
name. This also extends to function arguments in order to maintain simplicity.

.. note::
   Since this plugin is mostly focused on manipulating generic menus (menu type 1011), functions
   that do not specify menu type are inherently for generic menus. To use functions of other menu
   types, you need to use ``ByMenuType`` functions


.. toctree::
   :caption: Contents
   :maxdepth: 2

   value
   handlers
   lists
   scrolling
   ancestry
