# Interesting things on Flutter

### Layoutbuilder

```dart
 Widget _bottomNav(BuildContext context){
    return Container(
      height: 55,
      margin: const EdgeInsets.only(left: 25, right: 25, bottom: 25),
      decoration: BoxDecoration(
        borderRadius: BorderRadius.circular(8.0),
        color: Colors.blueAccent
      ),
      child: LayoutBuilder(builder: (context, constraints){
        double barWidth = constraints.maxWidth / _icons.length;
        return Obx((){
          return Row(
            children: _icons.map((icon) {
              int iconIndex = _icons.indexOf(icon);
              bool isSelected = (controller.bottomBarIndex.value == iconIndex);
              return SizedBox(
                width: barWidth,
                child: GestureDetector(
                  onTap: (){
                    controller.bottomBarIndex.value = iconIndex;
                  },
                  child: Icon(icon, color: isSelected ? Colors.white : Colors.black,),
                ),
              );
            }).toList(),
          );
        });
      }),
    );
  }
```

This code has some awesome calculation to make things dynamic. 

**Idea is:** `LayoutBuilder` is **dynamic** widget right so, in order to response along with the icon then they should know how much they need to fit in so that everyone place perfectly. If we do `constraints.maxWidth / _icons.length;` means that they will get exactly the width icon needed. Damn....
