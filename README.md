# Developer Docs
#### Module Structure
**Directories**
 
A module is organized in important directories. Those contain the business logic; having a look at them should make you understand the purpose of the module.
 
- data/ : contains data (required records like scheduled actions,email
- templates,sequence etc)(.xml)
- models/ : models definition (Class)
- controllers/ : contains controllers (HTTP routes)
- views/ : contains the views and templates
- static/ : contains the web assets, separated into css/, js/, img/, lib/, etc.
- menu/ : contains the menus of all models
- security/ : contains access rights, groups, record rules
 
Other optional directories compose the module.
 
- wizard/ : regroups the transient models (models.TransientModel) and their views
- report/ : contains the printable reports and models based on SQL views. Python objects and XML views are included in this directory
- i18n/ : contains the translation data (.po, .pot)
- demo/ : contains demo data (.xml)
- tests/ : contains the Python tests
 
**Files**
 
`__init__.py`
 
Contains python `file's` packages.
If required you can also define pre_init_hook, post_init_hook functions.
 
`__manifest__.py`
 
In Odoo, the `manifest` file tells essential information about the module, including its name, version, description, author, dependencies, and data files.
<br/>
#### File Naming
File naming is important to quickly find information through all odoo addons. This section explains how to name files in a standard odoo module. As an example we use a OpenEduCat application. It holds two main models *op.course* and *op.student*.
 
Concerning models, split the business logic by sets of models belonging to a same main model. Each set lies in a given file named based on its main model. If there is only one model, its name is the same as the module name. Each inherited model should be in its own file to help understanding of impacted models.
 
    addons/plant_nursery/
    |-- models/
    |	|-- plant_nursery.py (first main model)
    |	|-- plant_order.py (another main model)
    |	|-- res_partner.py (inherited Odoo model)
 
Concerning *security*, three main files should be used:
- First one is the definition of access rights done in a `ir.model.access.csv` file.
- User groups are defined in `<module>_groups.xml`.
- Record rules are defined in `<model>_security.xml `.
 
    addons/plant_nursery/
        |-- security/
        |	|-- ir.model.access.csv
        |	|-- plant_nursery_groups.xml
        |	|-- plant_nursery_security.xml
        |	|-- plant_order_security.xml
 
Concerning views, backend views should be split like models and suffixed by `_views.xml `. Backend views are list, form, kanban, activity, graph, pivot, .. views. To ease split by model in views main menus not linked to specific actions may be extracted into an optional `<module>_menus.xml` file. Templates (QWeb pages used notably for portal/website display) are put in separate files named `<model>_templates.xml `.
 
    addons/plant_nursery/
    |-- views/
    |	| -- plant_nursery_menus.xml (optional definition of main menus)
    |	| -- plant_nursery_views.xml (backend views)
    |	| -- plant_nursery_templates.xml (portal templates)
    |	| -- plant_order_views.xml
    |	| -- plant_order_templates.xml
    |	| -- res_partner_views.xml
 
 
Concerning data, split them by purpose (demo or data) and main model. Filenames will be the main_model name suffixed by `_demo.xml` or `_data.xml `. For instance for an application having demo and data for its main model as well as subtypes, activities and mail templates all related to mail module:
 
    addons/plant_nursery/
    |-- data/
    |	|-- plant_nursery_data.xml
    |	|-- plant_nursery_demo.xml
    |	|-- mail_data.xml
 
Concerning controllers, generally all controllers belong to a single controller contained in a file named `<module_name>.py` . An old convention in Odoo is to name this file `main.py` but it is considered as outdated. If you need to inherit an existing controller from another module do it in `<inherited_module_name>.py` . For example adding portal controller in an application is done in `portal.py`.
 
    addons/plant_nursery/
    |-- controllers/
    |	|-- plant_nursery.py
    |	|-- portal.py (inheriting portal/controllers/portal.py)
    |	|-- main.py (deprecated, replaced by plant_nursery.py)
 
Concerning *static* files, Javascript files follow globally the same logic as python models. Each component should be in its own file with a meaningful name. For instance, the activity widgets are located in `activity.js` of mail module. Subdirectories can also be created to structure the *package* (see web module for more details). The same logic should be applied for the templates of JS widgets (static XML files) and for their styles (scss files). Donʼt link data (image, libraries) outside Odoo: do not use an URL to an image but copy it in the codebase instead.
 
Concerning wizards, naming convention is the same of for python models: `<transient>.py` and `<transient>_views.xml`. Both are put in the wizard directory. This naming comes from old odoo applications using the wizard keyword for transient models.
 
    addons/plant_nursery/
    |-- wizard/
    |	|-- make_plant_order.py
    |	|-- make_plant_order_views.xml
 
Concerning statistics reports done with python/SQL views and classic views naming is the following:
 
    addons/plant_nursery/
    |-- report/
    |	|-- plant_order_report.py
    |	|-- plant_order_report_views.xml
 
Concerning *printable reports* which contain mainly data preparation and Qweb templates naming is the following:
 
    addons/plant_nursery/
    |-- report/
    |	|-- plant_order_reports.xml (report actions, paperformat, etc.)
    |	|-- plant_order_templates.xml (xml report templates)
 
The complete tree of our Odoo module therefore looks like:
 
    addons/plant_nursery/
    |-- __init__.py
    |-- __manifest__.py
    |-- controllers/
    |	|-- __init__.py
    |	|-- plant_nursery.py
    |	|-- portal.py
    |-- data/
    |	|-- plant_nursery_data.xml
    |	|-- plant_nursery_demo.xml
    |	|-- mail_data.xml
    |-- models/
    |	|-- __init__.py
    |	|-- plant_nursery.py
    |	|-- plant_order.py
    |	|-- res_partner.py
    |-- report/
    |	|-- __init__.py
    |	|-- plant_order_report.py
    |	|-- plant_order_report_views.xml
    |	|-- plant_order_reports.xml (report actions, paperformat, ...)
    |	|-- plant_order_templates.xml (xml report templates)
    |-- security/
    |	|-- ir.model.access.csv
    |	|-- plant_nursery_groups.xml
    |	|-- plant_nursery_security.xml
    |	|-- plant_order_security.xml
    |-- static/
    |	|-- img/
    |	|	|-- my_little_kitten.png
    |	|	|-- troll.jpg
    |	|-- lib/
    |	|	|-- external_lib/
    |	|-- src/
    |	|	|-- js/
    |	|	|	|-- widget_a.js
    |	|	|	|-- widget_b.js
    |	|	|-- scss/
    |	|	|	|-- widget_a.scss
    |	|	|	|-- widget_b.scss
    |	|	|-- xml/
    |	|	|	|-- widget_a.xml
    |	|	|	|-- widget_a.xml
    |-- views/
    |	|-- plant_nursery_menus.xml
    |	|-- plant_nursery_views.xml
    |	|-- plant_nursery_templates.xml
    |	|-- plant_order_views.xml
    |	|-- plant_order_templates.xml
    |	|-- res_partner_views.xml
    wizard/
    	|--make_plant_order.py
    	|--make_plant_order_views.xml

# Models

## Class

class name must be in PascalCase and must be relavant to the tbale name eg. for 'op.course' it will be OpCourse. for new models must follow below code.must have _name and _description.

```python
from odoo import models


class OpCourse(models.Model):
	_name = "op.course"
	_description = "Course"
```

for inherited models must follow below code.must have _inherit.

```python
from odoo import models


class OpSubject(models.Model):
	_inherit = "op.subject"
```

## Compute Field

For Compute Field This Things Must Be Followed, function name must start with _compute_, there must be loop over the record sets, some value must be assign to field.

```python
full_name = fields.Char("Full Name", compute="_compute_full_name")

@api.depends('first_name', 'last_name')
def _compute_full_name(self):
	for record in self:
		record.full_name = (record.first_name or '') + (record.last_name or '')
```

# Think extendable
Functions and methods should not contain too much logic: having a lot of small and simple methods is more advisable than having few large and complex methods. A good rule of thumb is to split a method as soon as it has more than one responsibility (see http://en.wikipedia.org/wiki/Single_responsibility_principle).

Hardcoding a business logic in a method should be avoided as it prevents to be easily extended by a submodule.

```python
# do not do this
# modifying the domain or criteria implies overriding whole method
def action(self):
    ...  # long method
    partners = self.env['res.partner'].search(complex_domain)
    emails = partners.filtered(lambda r: arbitrary_criteria).mapped('email')

# better but do not do this either
# modifying the logic forces to duplicate some parts of the code
def action(self):
    ...
    partners = self._get_partners()
    emails = partners._get_emails()

# better
# minimum override
def action(self):
    ...
    partners = self.env['res.partner'].search(self._get_partner_domain())
    emails = partners.filtered(lambda r: r._filter_partners()).mapped('email')
```
The above code is over extendable for the sake of example but the readability must be taken into account and a tradeoff must be made.

Also, name your functions accordingly: small and properly named functions are the starting point of readable/maintainable code and tighter documentation.

This recommendation is also relevant for classes, files, modules and packages. (See also http://en.wikipedia.org/wiki/Cyclomatic_complexity)

# Symbols and Conventions
Model name (using the dot notation, prefix by the module name) :
- When defining an Odoo Model : use singular form of the name (res.partner and sale.order instead of res.partnerS and saleS.orderS)

- When defining an Odoo Transient (wizard) : use <related_base_model>.<action> where related_base_model is the base model (defined in models/) related to the transient, and action is the short name of what the transient do. Avoid the wizard word. For instance : account.invoice.make, project.task.delegate.batch, …

- When defining report model (SQL views e.i.) : use <related_base_model>.report.<action>, based on the Transient convention.

Odoo Python Class : use camelcase (Object-oriented style).

```python
class AccountInvoice(models.Model):
    ...
```

Variable name :
- use camelcase for model variable

- use underscore lowercase notation for common variable.

- suffix your variable name with _id or _ids if it contains a record id or list of id. Don’t use partner_id to contain a record of res.partner

```python
Partner = self.env['res.partner']
partners = Partner.browse(ids)
partner_id = partners[0].id
```

- One2Many and Many2Many fields should always have _ids as suffix (example: sale_order_line_ids)

- Many2One fields should have _id as suffix (example : partner_id, user_id, …)

- Method conventions
	- Compute Field : the compute method pattern is _compute_<field_name>

	- 	Search method : the search method pattern is _search_<field_name>

	- 	Default method : the default method pattern is _default_<field_name>

	- 	Selection method: the selection method pattern is _selection_<field_name>

	- 	Onchange method : the onchange method pattern is _onchange_<field_name>

	- 	Constraint method : the constraint method pattern is _check_<constraint_name>

	- 	Action method : an object action method is prefix with action_. Since it uses only one record, add self.ensure_one() at the beginning of the method.

- In a Model attribute order should be
	- Private attributes (_nam- e, _description, _inherit, _sql_constraints, …)

	- 	Default method and default_get

	- 	Field declarations

	- 	Compute, inverse and search methods in the same order as field declaration

	- 	Selection method (methods used to return computed values for selection fields)

	- 	Constrains methods (@api.constrains) and onchange methods (@api.onchange)

	- 	CRUD methods (ORM overrides)

	- 	Action methods

	- 	And finally, other business methods.

```python
class Event(models.Model):
    # Private attributes
    _name = 'event.event'
    _description = 'Event'

    # Default methods
    def _default_name(self):
        ...

    # Fields declaration
    name = fields.Char(string='Name', default=_default_name)
    seats_reserved = fields.Integer(string='Reserved Seats', store=True
        readonly=True, compute='_compute_seats')
    seats_available = fields.Integer(string='Available Seats', store=True
        readonly=True, compute='_compute_seats')
    price = fields.Integer(string='Price')
    event_type = fields.Selection(string="Type", selection='_selection_type')

    # compute and search fields, in the same order of fields declaration
    @api.depends('seats_max', 'registration_ids.state', 'registration_ids.nb_register')
    def _compute_seats(self):
        ...

    @api.model
    def _selection_type(self):
        return []

    # Constraints and onchanges
    @api.constrains('seats_max', 'seats_available')
    def _check_seats_limit(self):
        ...

    @api.onchange('date_begin')
    def _onchange_date_begin(self):
        ...

    # CRUD methods (and name_search, _search, ...) overrides
    def create(self, values):
        ...

    # Action methods
    def action_validate(self):
        self.ensure_one()
        ...

    # Business methods
    def mail_user_confirm(self):
        ...
```
