import os
import sys
import rtconfig

from rtconfig import RTT_ROOT

sys.path = sys.path + [os.path.join(RTT_ROOT, 'tools')]
from building import *

# set RTAK_ROOT
# if not os.getenv("RTAK_ROOT"):
#     RTAK_ROOT="../edge-ai/RTAK/rt_ai_lib"
    
TARGET = 'rtthread.' + rtconfig.TARGET_EXT

DefaultEnvironment(tools=[])
env = Environment(tools = ['mingw'],
    AS = rtconfig.AS, ASFLAGS = rtconfig.AFLAGS,
    CC = rtconfig.CC, CCFLAGS = rtconfig.CFLAGS,
    CXX = rtconfig.CXX, CXXFLAGS = rtconfig.CXXFLAGS,
    AR = rtconfig.AR, ARFLAGS = '-rc',
    LINK = rtconfig.LINK, LINKFLAGS = rtconfig.LFLAGS)
env.PrependENVPath('PATH', rtconfig.EXEC_PATH)
env['ASCOM'] = env['ASPPCOM']

Export('RTT_ROOT')
Export('rtconfig')


# prepare building environment
objs = PrepareBuilding(env, RTT_ROOT, has_libcpu = False)
# include RT-AK library
# objs.extend(SConscript(os.path.join(RTAK_ROOT, 'Sconscript')))
stack_size = 4096

stack_lds = open('link_stacksize.lds', 'w')
if GetDepend('__STACKSIZE__'): stack_size = GetDepend('__STACKSIZE__')
stack_lds.write('__STACKSIZE__ = %d;' % stack_size)
stack_lds.close()

# make a building
DoBuilding(TARGET, objs)
