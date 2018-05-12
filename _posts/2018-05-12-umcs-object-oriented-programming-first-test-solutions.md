---
theme: post
title: "UMCS Object Oriented Programming first test solutions"
categories: [umcs]
tags: [programming, cpp]
---

# Exercise 1
```cpp{% raw %}
#include <algorithm>
#include <cmath>

class Polynomial {
 public:
  Polynomial(const int degree) {
    m_deg = degree;
    // polynomial of degree 0 has one coefficient, degree 1 two, etc.
    m_size = m_deg + 1;
    m_coeffs = new double[m_size];
    std::fill(m_coeffs, m_coeffs + m_size, 0.0);  // <algorithm>
  }

  Polynomial(const Polynomial& org) {
    m_deg = org.m_deg;
    m_size = org.m_size;
    m_coeffs = new double[m_size];
    std::copy(org.m_coeffs, org.m_coeffs + org.m_size,
              m_coeffs);  // <algorithm>
  }

  ~Polynomial() {
    delete[] m_coeffs;
  }

  double& access(const int n) {
    if (n >= m_size) {
      const int degree = n;
      const int size = n + 1;
      double* const coeffs = new double[size];
      for (int i = 0; i < size; ++i) {
        coeffs[i] = i < m_size ? m_coeffs[i] : 0.0;
      }
      delete[] m_coeffs;

      m_deg = degree;
      m_size = size;
      m_coeffs = coeffs;
    }

    return m_coeffs[n];
  }

  double constAccess(const int n) const {
    if (n >= m_size) return 0.0;

    return m_coeffs[n];
  }

  double value(const double x) const {
    double res = 0;
    for (int i = 0; i < m_size; ++i) {
      res += pow(x, i) * m_coeffs[i];  // <cmath>
    }

    return res;
  }

 private:
  int m_deg = 0;  // degree

  double* m_coeffs = nullptr;  // coefficients
  int m_size = 0;
};

{% endraw %}```


# Exercise 2
## singleton.hpp
```cpp{% raw %}
namespace singleton {
class Singleton {
 public:
  static Singleton* getInstance();
  static void deleteInstance();

 private:
  Singleton* m_instance = nullptr;

  Singleton();
  ~Singleton();

  Singleton(const Singleton&) = delete;
};
}
{% endraw %}```

## singleton.cpp

```cpp{% raw %}
#include "ex2.hpp"

namespace singleton {
static Singleton* Singleton::getInstance() {
  if (!m_instance) {
    m_instance = new Singleton;
  }

  return m_instance;
}

static void Singleton::deleteInstance() {
  delete m_instance;
}

Singleton::Singleton() {}

Singleton::~Singleton() {}
}
{% endraw %}```


# Exercise 3
```cpp{% raw %}
#include <sstream>
#include <cmath>
#include <fstream>
#include <iostream>  // for tests only
#include <string>

class Shape {
 public:
  struct Point {
    double x;
    double y;
  };

  virtual bool save(const std::string& path) const = 0;
  virtual ~Shape() {}

 protected:
  virtual std::string info() const = 0;
};

bool Shape::save(const std::string& path) const {
  std::ofstream file(path);

  if (!file.good()) return false;

  file << info() << std::endl;

  return true;
}

double distance(const Shape::Point p1, const Shape::Point p2) {
  return sqrt(pow(p2.x - p1.x, 2) + pow(p2.y - p1.y, 2));
}

class Triangle : public Shape {
 public:
  Triangle(const Point p1, const Point p2, const Point p3) {
    m_p1 = p1;
    m_p2 = p2;
    m_p3 = p3;
  }

  virtual bool save(const std::string& path) const override {
    return Shape::save(path);
  }

 protected:
  virtual std::string info() const override {
    std::stringstream ss;
    ss << "Triangle" << std::endl;
    ss << points() << std::endl;
    ss << circ() << std::endl;
    ss << area() << std::endl;

    return ss.str();
  }

 private:
  double area() const {
    const double a = distance(m_p1, m_p2);
    const double b = distance(m_p2, m_p3);
    const double c = distance(m_p3, m_p1);
    const double p = (a + b + c) / 2.0;
    return sqrt(p * (p - a) * (p - b) * (p - c));
  }

  double circ() const {
    return distance(m_p1, m_p2) + distance(m_p2, m_p3) + distance(m_p3, m_p1);
  }

  std::string points() const {
    std::stringstream ss;
    ss << m_p1.x << " " << m_p1.y << std::endl;
    ss << m_p2.x << " " << m_p2.y << std::endl;
    ss << m_p3.x << " " << m_p3.y;

    return ss.str();
  }

 private:
  Point m_p1;
  Point m_p2;
  Point m_p3;
};

class Circle : public Shape {
 public:
  Circle(Point p, const double r) {
    m_p = p;
    m_r = r;
  }

  virtual bool save(const std::string& path) const override {
    return Shape::save(path);
  }

 protected:
  virtual std::string info() const override {
    std::stringstream ss;
    ss << "Circle" << std::endl;
    ss << m_p.x << " " << m_p.y << std::endl;
    ss << m_r << std::endl;
    ss << M_PI * 2.0 * m_r << std::endl;  // circ
    ss << M_PI * m_r * m_r << std::endl;  // area

    return ss.str();
  }

 private:
  Point m_p;
  double m_r;
};

void f(Shape** shapes, const int size, const std::string& base_path) {
  for (int i = 0; i < size; ++i) {
    shapes[i]->save(base_path + std::to_string(i));
  }
}

/* -------------------------------------------------------------------------- */
// testing
int main() {
  Shape::Point p1;
  p1.x = 10;
  p1.y = 10;
  Shape::Point p2;
  p2.x = 15;
  p2.y = 10;
  Shape::Point p3;
  p3.x = 12.5;
  p3.y = 15;
  Shape::Point p4;
  p4.x = 10;
  p4.y = 20;

  const int size = 3;
  Shape** shapes = new Shape*[size];
  shapes[0] = new Triangle(p1, p2, p3);
  shapes[1] = new Triangle(p1, p2, p4);
  shapes[2] = new Circle(p3, 10);
  
  f(shapes, size, "shape");
  
  delete[] shapes;

  return 0;
}
{% endraw %}```
