Press *F5* to reload your Texture Atlas if you've made any changes to your source images. This is a nice way to make incremental adjustments to your skin widgets without having to test them in game.

Parenting is a libGDX feature since 1.9.9. This helps clean up definitions of widgets when they can be inherited from a parent style with only minor additions. This feature has been added to the top of the style properties of each widget in Skin composer. This even allows for parenting across classes (ex. TextButton style can have a Button style parent.

Importing an existing project is a rudimentary technique to unpack an existing skin's texture atlas. See the data folder after saving an imported project.

Skin Composer's temporary folder and settings can be accessed for debugging by following the links in *Project >> Settings*