=======================
Country Zip ES Scenario
=======================

Imports::
    >>> import datetime
    >>> from dateutil.relativedelta import relativedelta
    >>> from decimal import Decimal
    >>> from operator import attrgetter
    >>> from proteus import config, Model, Wizard
    >>> today = datetime.date.today()

Create database::

    >>> config = config.set_trytond()
    >>> config.pool.test = False

Install account_invoice::

    >>> Module = Model.get('ir.module')
    >>> module, = Module.find([('name', '=', 'country_zip_es')])
    >>> Module.install([module.id], config.context)
    >>> Wizard('ir.module.install_upgrade').execute('upgrade')

Load all spanish banks::

    >>> Wizard('load.country.zips').execute('accept')

Ensure that banks are loaded::

    >>> CountryZip = Model.get('country.zip')
    >>> czip, = CountryZip.find([('zip', '=', '25001')])
    >>> czip.city
    u'Lleida'
    >>> czip.country.code
    u'ES'
