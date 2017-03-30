{% gist 264569c2d3794e69af5002beee5649ec %}


### []( 
```cpp
#include <QApplication>
#include <QDesktopWidget>
#include "mainwindow.h"

int main(int argc, char* argv[]) {
  // Exactly one application is needed for each program, no matter how many
  // windows are displayed. QApplication is used with all GUI apps.
  QApplication a(argc, argv);

  QRect r = QApplication::desktop()->availableGeometry();

  int width = r.width() / 2 - 100;
  int height = r.height() - 100;

  if (bool fullscreen = false) {
    width = r.width();
    height = r.height();
  }

  std::cout << "screen dimensions: " << width << " x " << height << std::endl;


  MainWindow w(width, height);
  w.setFixedSize(width, height);

  // without .show() program will run with no interface, doing nothing
  w.show();

  return a.exec();
}

```
