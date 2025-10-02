# C++ Generator

The **C++ generator** is currently the most developed one.
The generated code is based on the **C++11 standard** and can be compiled with both **GCC (g++)** and **Clang (clang++)**.

---

## Serialization Systems Supported

* **DBus**
* **Stream**
* **JSON** (partial)

---

## Connectors

* **Async call & return**
* **Dispatcher**
* **Distributor**
* **Logger**

---

## Supported Links

* **TCP server & TCP client** (Linux Sockets)
* **UDP server & UDP client** (Linux Sockets)
* **DBus server & DBus client** → [DBus](https://www.freedesktop.org/wiki/Software/dbus/)
* **ZeroMQ server & client** → [ZeroMQ](https://zeromq.org/)
* **HTTP server & client** (Linux Sockets + [Atomizes](https://github.com/tinfoilboy/atomizes))
* **HTTPS server & client** (MbedTLS + [Atomizes](https://github.com/tinfoilboy/atomizes))

---

## Extras

* Direct **calls to interfaces**
* **Terminal** access to interfaces
* Integrated **logging system**
* [**SWIG** support](http://www.swig.org/) → Python bindings
