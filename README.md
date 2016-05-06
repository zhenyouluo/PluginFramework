# PluginFramework
A simple C++ framework for Plugins (interface + factory) loadable from shared
libraries

Usage:
1) Define your interface....

class someInterface {
  public:
    virtual blah iface(blah) = 0;
    virtual ~someInterface() {};
    someInterface(someArgs) {};
};
 

2) Define implementations of that interface.  In that, use the PLUGIN()
and PLUGIN_DEFS() macros.  PLUGIN declares the class (inheriting from the
interface via some other internal stuff).  PLUGIN_DEFS declares/defines
some magic registration objects (anda factory)

#include <Plugin>
#include <someInterface>

PLUGIN(someInteface, someImplementationName) {
  public:
    virtual blah iface(blah) { ... };
    ~someImplementation() {...};
    someImplementation(someargs) { ... };
};
PLUGIN_DEFS(someInterface, someImplementation);

3) Write a consumer.  Somewhere in the global scope in the actual process
you'll need to use PLUGIN_BASE()

#include <Plugin>
PLUGIN_BASE()

    ...
    try {
        someInterface *i = Plugin<someInterface,someargs>::getPlugin(
            "name", args...);
    } catch (std::runtime_error &e) {
	cerr << "Failed to load plugin: " << e.what() << endl;
    }

