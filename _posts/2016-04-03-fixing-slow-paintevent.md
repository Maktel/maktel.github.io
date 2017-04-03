---
title: "Fixing problems with slow paintEvent() in Qt"
categories: [qt]
tags: [fix, c++, graphics]
---

# [](#intro)Problem statement

I have been tinkering with my Qt drawing application, adding sliders to control zoom level. After fighting with slots and signals, I have managed to finally make the slider work. Everything worked, except... sometimes app felt unresponsive and slow -- but only when high level of zoom was set. Testing revealed paintEvent() function, which in turn calls image.scaled(), was to blame.


# [](#details)Details

My class responsible for creating window is called with 2 parameters: `const int width` and `const int height`. They don't change through life of the application and are used to set size of the window, as well as an image, which is used by all drawing functions as a canvas (I will use both words interchangeably). Since these parameters are constant, I had to find a way of zooming in the image to see drawing in greater details. Third constant was introduced, `double scale`; `paintEvent()` and `mousePressEvent()` make use of it -- the former to scale canvas down and the latter to correctly register mouse presses on a scaled image.

Let's take a look at `paintEvent()`:

```c++
// overloaded function of QWidget
void MainWindow::paintEvent(QPaintEvent*) {
  QPainter painter(this);
  painter.drawImage(0, 0, image.scaled(QSize(scale * width, scale * height)));
}
```

Basically, every time a widget is changed, moved or resized, `update()` function is called, and in turn also `paintEvent()`. When there are little to no changes, my function works just fine, but when we add a slider to the widget (I didn't bother to create a separate class for it), moving slider is one of the actions that indirectly call `image.scaled()`. As you can guess, it is a pretty costly operation, especially if done several times a second.


# [](solution)Solution

We have established frequent image scalling is a problem that is slowing down application. I can think of several solutions:
1. **cache drawn image -- scale it only if it has been modified**. It is a solution I have opted for. It required adding only one function and replacing it for previous calls to `update()`.
```c++
// . . .
QImage image_scaled;
// . . .

void MainWindow::redrawImage() {
  image_scaled = image.scaled(QSize(scale * width, scale * height));
  update();
}

void MainWindow::paintEvent(QPaintEvent*) {
  QPainter painter(this);
  painter.drawImage(0, 0, image_scaled);
}
```
2. **remove scaling and just resize image**. Probably the best of all, since even after my fix, high zooms don't feel instantaneous like low ones. It should benefit especially when working with gradients, when the image is redrawn often. 
3. **separate slider from drawing functionality** (not sure about that one; may still require other changes)
