# Micropython + lvgl for M5Stack Core (16MB Flash)

This branch contains my modifications to lv_micropython to make it work well with M5Stack Core.

It also includes:

- todo: Various bits of code from Lobo's MPy port

**For information abound Micropython lvgl bindings please refrer to [lv_bindings/README.md](https://github.com/littlevgl/lv_bindings/blob/master/README.md)**

See also [Micropython + LittlevGL](https://blog.littlevgl.com/2019-02-20/micropython-bindings) blog post.
For questions and discussions - please use the forum: https://forum.lvgl.io/c/micropython

Original micropython README: https://github.com/micropython/micropython/blob/master/README.md

## Development Enviroment Setup

Follow
This follows the [Micropython ESP32 README](https://github.com/micropython/micropython/blob/master/ports/esp32/README.md) with some minor changes.

### Stage 1: Repo Setup

```bash
$ export ESP32_DEV_PREFIX=$HOME/esp32-dev
$ export MPY_DIR=$ESP32_DEV_PREFIX/lv_micropython
$ mkdir -p $MPY_DIR
$ git clone --recurse-submodules --remote-submodules https://github.com/lv_micropython $MPY_DIR
$ cd $MPY_DIR
$ git checkout m5stack-changes
$ cd $ESP32_DEV_PREFIX
```

### Stage 2: ESP-IDF Setup

```bash
$ export ESPIDF=$ESP32_DEV_PREFIX/src/github.com/espressif/esp-idf
$ export IDF_COMMIT_HASH=4c81978a3e2220674a432a588292a4c860eef27b # As of 2020/08/24
$ mkdir -p $ESPIDF
$ cd $ESPIDF
$ git clone --recurse-submodules --remote-submodules https://github.com/espressif/esp-idf.git $ESPIDF
$ git checkout $IDF_COMMIT_HASH
$ git submodule update --init --recursive
```

### Stage 3: Compile-Ready

```bash
$ cd $MPY_DIR/ports/esp32
$ python3 -m venv build-venv
$ source build-venv/bin/activate
$ pip install --upgrade pip
$ pip install -r $ESPIDF/requirements.txt
$ cd $ESPIDF
$ ./install.sh
$ . ./export.sh
```

### Stage 4: Compile

```bash
$ cd $MPY_DIR
$ make -C mpy-cross
$ make -C ports/esp32 BOARD=M5STACK_CORE
```

### Environment Reset

I'm not big on modifying $PATH. So, whenever I want to compile this firmware, I re-activate the venv and run `export.sh`.

```bash
$ export ESP32_DEV_PREFIX=$HOME/esp32-dev
$ export MPY_DIR=$ESP32_DEV_PREFIX/lv_micropython
$ export ESPIDF=$ESP32_DEV_PREFIX/src/github.com/espressif/esp-idf
$ source $MPY_DIR/ports/esp32/build-venv/bin/activate
$ . $ESPIDF/export.sh
$ cd $MPY_DIR
```
## More information

More info about LittlevGL: 
- Website https://littlevgl.com
- GitHub: https://github.com/littlevgl/lvgl

More info about lvgl Micropython bindings:
- https://github.com/littlevgl/lv_bindings/blob/master/README.md

Discussions about the Microptyhon binding: https://github.com/littlevgl/lvgl/issues/557

More info about the unix port: https://github.com/micropython/micropython/wiki/Getting-Started#debian-ubuntu-mint-and-variants

