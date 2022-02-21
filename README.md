# Expand/Collapse String Parameter
Example files for Expand / Collapse String Parameter video

## Snippets

### Disable When / Hide When Expressions

Used to control parameter visibility.

***Hide When Expression***
```
{ expanded == 1 }
```

***Hide When Expression (multiparm)***
```
{ expanded# == 1 }
```

### Toggle Expand

*Callback function that controls our parameter reference switching.*

```python
# Toggle Expand (single)
def toggle_expand(node):
    collapsed = node.parm("note_collapsed")
    expanded = node.parm("note_expanded")
    parms = [collapsed, expanded]

    if node.evalParm("expanded"):
        parms.reverse()
    parms[0].deleteAllKeyframes()
    parms[1].deleteAllKeyframes()
    parms[1].set(parms[0])
    
toggle_expand(kwargs["node"])
``` 

```python
# Toggle Expand (multiparm, Python 2.7+)
def toggle_expand(node, parm_index):
    collapsed = node.parm("note_collapsed" + str(parm_index))
    expanded = node.parm("note_expanded" + str(parm_index))
    parms = [collapsed, expanded]

    if node.evalParm("expanded" + str(parm_index)):
        parms.reverse()
    parms[0].deleteAllKeyframes()
    parms[1].deleteAllKeyframes()
    parms[1].set(parms[0])

toggle_expand(kwargs["node"], kwargs["script_multiparm_index"])
``` 

```python 
# Toggle Expand (multiparm, Python 3)
def toggle_expand(node, parm_index):
    collapsed = node.parm(f"note_collapsed{parm_index}")
    expanded = node.parm(f"note_expanded{parm_index}")
    parms = [collapsed, expanded]

    if node.evalParm(f"expanded{parm_index}"):
        parms.reverse()
    parms[0].deleteAllKeyframes()
    parms[1].deleteAllKeyframes()
    parms[1].set(parms[0])

toggle_expand(kwargs["node"], kwargs["script_multiparm_index"])
```

### Action Button

*Nice UI element that controls the state of the `expanded` toggle.*

```python
# Action Button
parm = kwargs["node"].parm("expanded")
parm.set(1)
parm.pressButton()
```

```python
# Action Button (multiparm, Python 2.7+)
idx = kwargs["script_multiparm_index"]
parm = kwargs["node"].parm("expanded" + str(idx))
parm.set(0)
parm.pressButton()
```

```python
# Action Button (multiparm, Python 3)
idx = kwargs["script_multiparm_index"]
parm = kwargs["node"].parm(f"expanded{idx}")
parm.set(1)
parm.pressButton()
```

### Multiparm Default Channel Reference
```
chs("note_expanded#")
```
