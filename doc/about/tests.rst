﻿.. _tests:

================================
Tests and Validation
================================


Unit Tests
========================

Test Suite
--------------------------------------
pandapower is tested with pytest. There are currently over 220 tests testing all kinds of pandapower functionality. The tests also include automatic validation of 
pandapower results from power flow or short circuit calculations against commercial software, to ensure that the implementation is correct.

The complete test suite can be run with: ::

        import pandapower.test
        pandapower.test.run_all_tests()
    
If all packages are installed correctly, all tests should pass.

Continous Integration Testing
--------------------------------------
The tests are continously carried out with Travis CI in Python 2.7, 3.4, 3.5 and 3.6:

.. image:: https://travis-ci.org/lthurner/pandapower.svg?branch=develop
    :target: https://travis-ci.org/lthurner/pandapower

.. image:: https://img.shields.io/pypi/pyversions/pandapower.svg
    :target: https://pypi.python.org/pypi/pandapower

The test coverage rate is checked with codecov, code quality with codacy:

.. image:: https://codecov.io/github/lthurner/pandapower/coverage.svg?branch=develop
   :target: https://codecov.io/github/lthurner/pandapower?branch=master

.. image:: https://api.codacy.com/project/badge/Grade/5d749ed6772e47f6b84fb9afb83903d3
    :target: https://www.codacy.com/app/lthurner/pandapower/dashboard

.. _validate:

Model and Loadflow Validation
=============================
To ensure that pandapower loadflow results are correct, all pandapower element behaviour is tested against DIgSILENT PowerFactory or PSS Sincal. 

There is a result test for each of the pandapower elements that checks loadflow results in pandapower against results from a commercial tools. 
The results are compared with the following tolerances:

.. tabularcolumns:: |l|l|
.. csv-table:: 
   :file: tolerances.csv
   :delim: ;
   :widths: 30, 30

Example: Transformer Model Validation
--------------------------------------

To validate the pandapower transformer model, a transformer is created with the same parameters in pandapower and PowerFactory. To test all aspects of the model we use a transformer with

    - both iron and copper losses > 0
    - nominal voltages that deviate from the nominal bus voltages at both sides
    - an active tap changer
    - a voltage angle shift > 0

We use a transformer with the following parameters:

    - vsc_percent= 5.0
    - vscr_percent = 2.0
    - i0_percent = 0.4
    - pfe_kw = 2.0
    - sn_kva = 400
    - vn_hv_kv = 22
    - vn_lv_kv = 0.42
    - tp_max = 10
    - tp_mid = 5
    - tp_min = 0
    - tp_st_percent = 1.25
    - tp_side = "hv"
    - tp_pos = 3
    - shift_degree = 150

To validate the in_service parameter as well as the transformer switch element, we create three transformers in parallel: one in service, on out of service and one with an open switch in open loop operation.
All three transformers are connected to a 20kV / 0.4 kV bus network. The test network then looks like this:

.. image:: ../pics/validation/test_trafo.png
	:width: 10em
	:align: center
    
The loadflow result for the exact same network are now compared in pandapower and PowerFactory. It can be seen that both bus voltages:

.. image:: ../pics/validation/validation_bus.png
	:width: 20em
	:align: center

and transformer results:

.. image:: ../pics/validation/validation_trafo.png
	:width: 40em
	:align: center

match within the margins defined above.

All Test Networks
-------------------------------------

There is a test network for the validation of each pandapower element in the same way the transformer model is tested.

The PowerFactory file containing all test networks can be downloaded :download:`here  <../../pandapower/test/test_files/test_results.pfd>`.
The correlating pandapower networks are defined in result_test_network_generatory.py in the pandapower/test module.
The tests that check pandapower results against PowerFactory results are located in pandapower/test/test_results.py.

**line**
 
.. image:: ../pics/validation/test_line.png
	:width: 12em
	:align: center

**load and sgen**

.. image:: ../pics/validation/test_load_sgen.PNG
	:width: 8em
	:align: center

**trafo**

.. image:: ../pics/validation/test_trafo.png
	:width: 10em
	:align: center    
    
**trafo3w**

.. image:: ../pics/validation/test_trafo3w.PNG
	:width: 20em
	:align: center   

**ext_grid**

.. image:: ../pics/validation/test_ext_grid.PNG
	:width: 10em
	:align: center   
    
**shunt**

.. image:: ../pics/validation/test_shunt.PNG
	:width: 8em
	:align: center  

**gen**

.. image:: ../pics/validation/test_gen.PNG
	:width: 20em
	:align: center  
    
**impedance**

.. image:: ../pics/validation/test_impedance.PNG
	:width: 10em
	:align: center  
    
**ward**

.. image:: ../pics/validation/test_ward.png
	:width: 8em
	:align: center  
    
**xward**

.. image:: ../pics/validation/test_xward.PNG
	:width: 20em
	:align: center  

**switch**

.. image:: ../pics/validation/test_bus_bus_switch.PNG
	:width: 40em
	:align: center  

    
