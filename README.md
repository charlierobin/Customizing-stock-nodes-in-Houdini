# Customizing stock nodes in Houdini
 
A slight enhancement to the (excellent) little Toadstorm Nerdblog Houdini tutorial at: https://www.toadstorm.com/blog/?p=1012, titled: **Customizing stock nodes in Houdini** (published by toadstorm on 05/18/2023).

That tutorial goes through a way of attaching a callback handler to a node’s parameter.

The example extends the base (stock) Color SOP: changing the color value parameter of the node updates the color of the node in the network view.

![Screenshot 2026-01-21 at 12 17 53](https://github.com/user-attachments/assets/310a2c40-e4e2-41dd-8e93-965bcde7961a)

This is done by providing Houdini with Python scripts named with:

(1) the name of the node whose events you want to look at

(2) an underscore

(3) the name of the event itself

...like so:

nodeName_eventName.py

(Remember that all names are case sensitive.)

The event names above are the ones from `Edit Operator Type Properties` window (`Scripts` tab):

```
PreFirstCreate
OnCreated
OnLoaded
OnDeleted
PostLastDelete
OnInputChanged
OnNameChanged
```

I think there are a couple of places where these scripts can be put for Houdini to find them, but (for better or worse) this is where I always put mine:

`/Users/charlie/Library/Preferences/houdini/18.5/scripts/sop/color_OnCreated.py`
`/Users/charlie/Library/Preferences/houdini/18.5/scripts/sop/color_OnLoaded.py`

Then, within those scripts, you further examine the information provided by Houdini to determine what you want to handle (`kwargs`).

For this example, the event is `hou.nodeEventType.ParmTupleChanged`.

But there are plenty of others. Check out the Houdini Python docs: 

https://www.sidefx.com/docs/houdini/hom/hou/nodeEventType.html

Within the callback handler, the new color parameter is applied to the node color.

All well and good. However, one problem I noted is that the node color is not automatically set the first time it is dropped into the Network view.

Any subsequent updates to the color work fine, ie: clicking on the node and then adjusting the color picker(s), but not that initial one.

Therefore when (assuming your default color is white – (1.0,1.0,1.0)) you drop a new color node, the initial color of the node in the network view remains its default, which is a kind of light grey. It’s only (if and) when you go and edit the color that the node color then updates.

So I just added the few lines necessary to the final example scripts (specifically, only the `OnCreated` handler) so that this now works the way you would expect it to.
