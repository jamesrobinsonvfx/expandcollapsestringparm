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
chs("note_collapsed#")
```

### UPDATE
Petr Zloty left a very handy comment on Vimeo demonstrating how 
you can expand/collapse a parm using the Action Button only by 
modifying the `parmTemplate`. This solution is simple and elegant,
and better overall for single parameters. However, this method will
not work for individual multiparms since modifying the `parmTemplate` 
for one modifies them all which is something I wanted to avoid.

```python
# current node
node = kwargs['node']

# current parameter
pt = kwargs["parmtuple"]

# its template
template = pt.parmTemplate()

# parameter's tags
tags = template.tags()

# invert collapsed tag
tags['editor'] = str(1-int(tags['editor']))

# invert icon
tags['script_action_icon'] = "KEYS_Up" if tags['script_action_icon'] == "KEYS_Down" else "KEYS_Down"

# set updated tags
template.setTags(tags)

# replace parameter template with updated tags
group = node.parmTemplateGroup()
group.replace(pt.name(), template)
node.setParmTemplateGroup(group)
```
