objects = \
    dbconnection_pool.o\


CC = g++
cc = g++

MYSQLPP_INSTLLED_DIR = /$(LIBS_DIR)/mysql++/installed

include_flags        = $(shell $(MYSQL_CONFIG_CMD) --include) \
					   -I$(LIBS_DIR)/mysql++/installed/include \
					   -I$(BASE_DIR)
lib_flags            = $(shell $(MYSQL_CONFIG_CMD) --libs_r)\
					   -L$(LIBS_DIR)/mysql++/installed/lib -lmysqlpp

cc_flags = -std=c++11 -ggdb -Wall -Werror $(optimization_flag)\
		   $(include_flags)

ln_flags = -lpthread $(lib_flags)

target = libdbadaptcher.a

.PHONY:
all: $(target)

$(target) : $(objects)
	ar rs $(target) $(objects)

%.o: %.cpp
	$(cc) $(cc_flags) -c $< -o $@

%.d: %.cpp
#此行用于调试	@set -e -x; \ #此行用于调试
	@rm -f $@; \
	$(cc) $(cc_flags) -MM $< > $@.$$$$;  \
	sed 's,\($*\)\.o[ :]*,\1.o $@ : ,g' < $@.$$$$ > $@;  \
	rm -f $@.$$$$

ifneq ($(MAKECMDGOALS),clean)
ifneq ($(MAKECMDGOALS),distclean)
-include $(objects:.o=.d)
endif
endif


.PHONY: clean distclean
clean:
	rm -f $(objects) $(objects:.o=.d) *.d *.d.* 
	rm -f $(target)

distclean: clean
