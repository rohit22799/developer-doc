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

# Multi Company Support

Model must have multi company support. for multi company support add following block in models.

```python
company_id = fields.Many2one('res.company', required=True, default=lambda self: self.env.company)
```

# Multi Website Support

if data of Model is published on data then model must have multi website support. also add publishable mixin to make content publish or unpublish. for multi website support add following block in models

```python
_inherit = ['website.published.multi.mixin']

website_id = fields.Many2one('website')
```


#XML  Guidelines #

**File Naming:** Use lowercase with underscores (_) for file names (e.g., sale_order_views.xml).
**Encoding: ** Always ensure the file begins with an XML declaration:
```xml
            <?xml version="1.0" encoding="UTF-8"?>
```
**Root Element: **Use < odoo > as the root element in every XML file except JS templates files .

### Formatting and Indentation

 - Use 4 spaces for indentation (no tabs).
 - Keep XML elements properly nested.
 - Align attributes of an XML tag vertically if the tag has multiple attributes:
```xml
<field name="name" 
       string="Order Name" 
       required="1"/>
```
 - Add a blank line between logical sections.

## View Definitions

- Define views within < record > elements using the model ir.ui.view:
```xml
			<record id="view_order_tree" model="ir.ui.view">
    			<field name="name">sale.order.tree</field>
    			<field name="model">sale.order</field>
    			<field name="arch" type="xml">
       			 <tree string="Sales Orders">
            			<field name="name"/>
            			<field name="partner_id"/>
            			<field name="amount_total"/>
       	 		</tree>
   			 </field>
			 </record>
```
**Naming Conventions**

    - Use meaningful and descriptive IDs (e.g., view__< model >_ _< type>).
    - name field should clearly describe the view, e.g., sale.order.tree.

 **Consistent Layout:**

    - Align and group fields logically.
    - Use **group**, **notebook**, or **page** elements to separate sections.

  **Attributes**

    - **String Attribute: **Always provide user-friendly names for strings in views.
    - **Context and Domain:** Keep domains and contexts simple. Use Python expressions only when necessary:
	```xml
			<field name="partner_id" domain="[('customer', '=', True)]"/>
			```
	- **Avoid hardcoding values; **use externalized IDs where possible.

 **Field Options:**

    - Use appropriate widgets (e.g., many2many_tags, boolean_toggle) and options (e.g., {'no_create': True}).

## Modularization

   - Define related model views in the same XML file.
   - Split unrelated views into separate files to improve maintainability.
   - Use meaningful filenames like sale_order_views.xml, sale_report_views.xml, etc.
   - Do not include inline CSS or JavaScript in XML views. Use separate SCSS or JS files for customizations.
   - Use groups attributes to restrict access to specific views:
   		<field name="groups">sales_team.group_sale_manager</field>

- Define menus in a logical hierarchy. Use separate XML files for menus if they are extensive.

##Defining Menus
Odoo organizes menu items in a three-level hierarchy:

**Top-Level Menu:** Represents the main section (e.g., Sales, Inventory).
    **Second-Level Menu:** Represents sub-sections or categories within the main menu (e.g., Orders, Products under Sales).
    **Third-Level Menu (Leaf):** Direct links to views (e.g., Quotations, Products, Stock Moves).

Menu items are typically defined in XML files within the menu directory of your module. Use the < menuitem > tag.

		<odoo>
    		<menuitem id="menu_main" name="Custom Module" sequence="1" />
    		<menuitem id="menu_sub" name="Submenu" parent="menu_main" sequence="1" />
    		<menuitem id="menu_leaf" name="Records" parent="menu_sub" action="action_custom_records" sequence="1" />
		</odoo>


- Use clear and concise names.
- Avoid overly technical terms unless necessary.
- Ensure the names reflect the purpose of the menu.
- Group related menu items under appropriate sections to make navigation intuitive.
- Assign the sequence attribute to control the order of menu items.
- Lower numbers are displayed first.
- Do not create redundant menu items pointing to the same action/view.
- Use security groups to control visibility if needed.
- Use the parent attribute to define hierarchical relationships.
- Top-level menus should not have a parent attribute.
- Leaf nodes (third-level menus) or Sub section (second-level menus) should link to specific actions (e.g., tree, form, kanban views).
- Use the groups attribute to restrict menu visibility to specific user groups.
```xml
<menuitem id="menu_admin_only" name="Admin Menu" groups="base.group_system" />
```
-	  To hide or modify default menus, inherit the menuitem or action record.
```xml
<record id="module.menu_id_existing" model="ir.ui.menu">
    <field name="active" eval="False" />
</record>
```

**Tree View Guidelines**

The tree (list) view is used to display a list of records in a table format.
Best Practices:

   1. Key Columns:
        - Display the most important fields first, like name, state, and date.
        - Use optional="hide" for less important fields.

    			<field name="name" string="Name"/>
    			<field name="state" string="Status"/>
    			<field name="create_date" string="Created On" optional="hide"/>

   2. Interactivity:

    - Use editable="top" or "bottom" to allow quick editing if required.
    - Use readonly="1" for non-editable fields.

   3. Actions:

    -  Enable create and delete options if appropriate.
    - Use decoration-* attributes for visual indicators.

			<field name="state" decoration-danger="state == 'cancel'" decoration-success="state == 'done'"/>

**Example:**

	<record id="module_name_model_name_tree" model="ir.ui.view">
    	<field name="name">model.name.tree</field>
    	<field name="model">model.name</field>
    	<field name="arch" type="xml">
        	<tree string="Records" editable="bottom" create="1" delete="1">
            	<field name="name" string="Name"/>
            	<field name="state" string="Status" decoration-danger="state == 'cancel'" decoration-success="state == 'done'"/>
            	<field name="create_date" string="Created On" optional="hide"/>
        	</tree>
    	</field>
	</record>

**Form View Guidelines**

The form view is used for viewing and editing individual records.

**Best Practices:**

   1. Logical Grouping:
        - Use group or notebook elements to logically group fields.

   2. Header:
        - Include a header for key actions like validation or state change.

				<header>
					<button name="action_confirm" string="Confirm" type="object" class="oe_highlight"/>
					<field name="state" widget="statusbar" statusbar_visible="draft,confirmed,cancelled"/>
				</header>

   3. Field Attributes:

    - Use readonly, required, or invisible attributes based on the field’s state or condition.

   4. Chatter:

    - Always include the chatter (message and follower fields) for models with discussions.

			<div class="oe_chatter">
    			<field name="message_follower_ids"/>
    			<field name="message_ids"/>
			</div>

**Example:**

	<record id="module_name_model_name_form" model="ir.ui.view">
    	<field name="name">model.name.form</field>
    	<field name="model">model.name</field>
    	<field name="arch" type="xml">
        	<form string="Record">
            	<header>
                	<button name="action_confirm" string="Confirm" type="object" class="oe_highlight"/>
                	<field name="state" widget="statusbar" statusbar_visible="draft,confirmed,cancelled"/>
            	</header>
            	<sheet>
                	<group>
                    	<field name="name" placeholder="Enter Name" required="1"/>
                    	<field name="description"/>
                	</group>
                	<notebook>
                    	<page string="Details">
                        	<group>
                            	<field name="state" readonly="1"/>
                            	<field name="create_date" readonly="1"/>
                        	</group>
                    	</page>
                	</notebook>
            	</sheet>
            	<div class="oe_chatter">
                	<field name="message_follower_ids"/>
                	<field name="message_ids"/>
            	</div>
        	</form>
    	</field>
	</record>

**Kanban View Guidelines**

The kanban view is used for visualizing records in a card-based layout.
To be used only when a picture view is required.

**Best Practices:**

   1. Cards Design:
        - Display key information prominently (e.g., name, state).
        - Use badges for status or type fields.

   2. Grouping:
        - Enable grouping by drag-and-drop if applicable.

    			<field name="group_by">state</field>

   3. Mobile-Friendly:

    - Use o_kanban_mobile class for responsiveness.

**Example:**

	<record id="module_name_model_name_kanban" model="ir.ui.view">
    	<field name="name">model.name.kanban</field>
    	<field name="model">model.name</field>
    	<field name="arch" type="xml">
        	<kanban>
            	<field name="name"/>
            	<field name="state"/>
            	<templates>
                	<t t-name="kanban-box">
                    	<div t-attf-class="oe_kanban_global_click">
                        	<strong><field name="name"/></strong>
                        	<div><span class="badge"><field name="state"/></span></div>
                    	</div>
                	</t>
            	</templates>
        	</kanban>
    	</field>
	</record>

**Graph View Guidelines**

The graph view is used for visualizing data trends and analytics.

**Best Practices:**

   1. Key Metrics:
        - Define key fields for rows, columns, and measures.

        		<field name="create_date" type="row"/>
        		<field name="state" type="col"/>
        		<field name="amount" type="measure"/>

   2. Chart Types:
        -  Choose the most appropriate chart type (bar, line, pie).

**Example:**

	<record id="module_name_model_name_graph" model="ir.ui.view">
    	<field name="name">model.name.graph</field>
    	<field name="model">model.name</field>
    	<field name="arch" type="xml">
        	<graph string="Statistics">
            	<field name="create_date" type="row"/>
            	<field name="state" type="col"/>
            	<field name="amount" type="measure"/>
        	</graph>
    	</field>
	</record>

**Pivot View Guidelines**

The pivot view is used for summarizing data with grouping and aggregation.

**Best Practices:**

   1. Key Fields:
        - Define fields for rows, columns, and measures.

        		<field name="state" type="row"/>
        		<field name="create_date" type="col"/>
        		<field name="amount" type="measure"/>

   2. Sample Data:
        - Use sample="1" to preload sample data for demonstration purposes.

**Example:**

	<record id="module_name_model_name_pivot" model="ir.ui.view">
    	<field name="name">model.name.pivot</field>
    	<field name="model">model.name</field>
    	<field name="arch" type="xml">
        	<pivot string="Analysis" sample="1">
            	<field name="state" type="row"/>
            	<field name="create_date" type="col"/>
            	<field name="amount" type="measure"/>
        	</pivot>
    	</field>
	</record>

**View Integration**

Once views are defined, they should be linked to an action:

	<record id="action_module_name_model_name" model="ir.actions.act_window">
    	<field name="name">Records</field>
    	<field name="res_model">model.name</field>
    	<field name="view_mode">tree,form,kanban,graph,pivot</field>
	</record>

### Qweb Templates

It helps us design clear, consistent, and maintainable reports for your module. 

1. Consistent File Structure:

    Keep your report templates in a dedicated directory (e.g., report/).
    Separate your XML file definitions (data) and QWeb templates (report).

2. Naming Conventions:

    Use meaningful and consistent names for id, name, and file names.

    	<record id="module_name_report_template" model="ir.actions.report">
        	<field name="name">My Report</field>
    	</record>

3. Localization:

    Wrap translatable text in _() to support multiple languages.

    	<t t-esc="'Customer: ' + object.partner_id.name"/>

4. Modular Templates:

    Break down large templates into reusable components with < t t-call> for easier maintenance.

5. Use CSS for Styling:

    Minimize inline styles. Use a CSS file linked via t-call-assets.

    	<t t-call-assets="web.report_assets_common" />

6. Responsive Design:

    Ensure reports look good on various devices (especially if sent as PDFs or viewed in the browser).


**Key Components of Report Templates**

1. Define the Report Action

	Reports are linked to models using the ir.actions.report model.
	**Example:**
		<odoo>
    		<data>
        		<record id="module_name_report_action" model="ir.actions.report">
            		<field name="name">My Custom Report</field>
            		<field name="model">model.name</field>
            		<field name="report_type">qweb-pdf</field>
            		<field name="report_name">module_name.report_template</field>
            		<field name="report_file">module_name.report_template</field>
            		<field name="print_report_name">'Report_' + object.name</field>
            		<field name="binding_model_id" ref="model_name_id"/>
        		</record>
    		</data>
		</odoo>


2. Create the QWeb Template

	Define the HTML structure of your report in QWeb, these are XML files placed in the Reports directory.

		<odoo>
    		<template id="report_template">
        		<t t-call="web.html_container">
            	<t t-set="o" t-value="doc" />
            	<t t-call="web.internal_layout">
                	<div class="page">
                    	<h2>Report: <t t-esc="o.name"/></h2>
                    	<p>Date: <t t-esc="o.create_date"/></p>
                    	<p>Customer: <t t-esc="o.partner_id.name"/></p>
                    	<p>Total: <t t-esc="o.amount_total"/></p>
                	</div>
            	</t>
        	</t>
    	</template>
		</odoo>

**Detailed Guidelines for Qweb Report Elements**

1. Use Odoo's Built-in Headers and Footers:

    - Use *web.internal_layout * for a consistent header/footer.

2. Custom Header/Footer:

    - Override the default layout for specific needs.

			<template id="report_template_custom" inherit_id="web.internal_layout">
    			<xpath expr="//div[@class='header']" position="replace">
        			<div class="header">
            			<p>Custom Header Content</p>
        			</div>
    			</xpath>
			</template>

3. Use a Table for Structured Data:

    - Align data rows and columns using < table> for clarity.
    - Use a consistent design for headers and rows.

4. Extract Reusable Parts and Use Conditioning:

    - Create separate templates for reusable sections like headers, footers, or summary tables.
    - Use < t t-call> to Reuse.
    - Use t-if for Conditions:
		- Apply conditions to display or hide parts of the report.

5. Assets and Styling
	- Include Common Assets:
		- Use Odoo's *report_assets_common* for default styles.
		- Add a custom CSS file if needed.

### Data Files

For Loading initial data (e.g., product categories, default configurations).

- Place data files in the data/ directory of your module. Use subfolders if necessary for organization (e.g., data/demo/ for demo data).Use descriptive filenames that indicate their purpose, such as:

    1. default_data.xml
    1. product_categories.xml
    1. res_groups.csv

- Wrap data records within an < odoo > tag and group them under a < data > tag.
```xml
<odoo>
    <data>
        <record id="product_category_food" model="product.category">
            <field name="name">Food</field>
        </record>
        <record id="product_category_beverages" model="product.category">
            <field name="name">Beverages</field>
        </record>
    </data>
</odoo>
```
- Use unique and meaningful id attributes for all records.
- Follow a naming convention like <module_name>_<description> (e.g., product_category_food).
- Use ref attributes to link to existing records instead of hardcoding IDs.
```xml
<field name="parent_id" ref="product.product_category_all" />
```
- Maintain a logical order of records, especially if some depend on others.
- Load related data files in the correct sequence in your __manifest__.py.

Place demo data files in the demo/ directory.
- Mark demo data appropriately in the __manifest__.py file.
```xml
'demo': [
    'demo/demo_data.xml',
],
```
Use demo data for development and testing only. Avoid mixing demo data with production-ready data.

Do not include sensitive data (e.g., passwords, tokens) in plain text in data files.

Avoid directly overwriting core records unless necessary.
Use the noupdate attribute to prevent Odoo from updating certain records during module upgrades.
```xml
<odoo>
    <data noupdate="1">
        <record id="module.main_company_record_id" model="res.company">
            <field name="name">My Company</field>
        </record>
    </data>
</odoo>
```


### **Email Templates**

They are defined within the **data** directory of an module. These templates are defined using the < record > tag with the model
**mail.template**.

**Key Fields in Email Template Definitions**

  1. name:
        The name of the email template.
        Example: 
			< field name="name">Invoice Email< /field>

  2. model_id:
        Specifies the model to which the template applies.
        Example:
			< field name="model_id" ref="account.model_account_move"/>

  3. subject:
        The subject of the email.
        Example: 
			< field name="subject">Invoice: ${object.name}</field>

  4. email_from:
        Specifies the sender's email address.
        Example: 
			< field name="email_from">${(user.email or '')|safe}</field>

  5. email_to, email_cc, reply_to:
        Specify recipients for the email.
        Example:
			< field name="email_to">${(object.partner_id.email or '')|safe}</field>
			< field name="reply_to">${(object.company_id.email or '')|safe}</field>

  6. body_html:

    The content of the email in HTML format.
    Example:

    		< field name="body_html" type="html">
     		 <![CDATA[
        	< p>Dear ${object.partner_id.name},< /p>
        	< p>Your invoice <b>${object.name}< /b> is ready.< /p>
        	< p>Thank you.< /p>
     		 ]]>
    		< /field>

  7. attachment_ids:

    Links attachments to the email.
    Example:

		< field name="attachment_ids" eval="[(6, 0, [ref('account.invoice_report_template_id')])]"/>


**Key Notes for Email Templates**

    1. Dynamic Content:
        	Use placeholders like ${object.<field_name>} for inserting dynamic content into email templates.

    2. Attachments:
        	Reports or other files can be dynamically attached to emails using the attachment_ids field.

    3. Inheritance:
        	Email templates can be extended or modified by inheriting them with <record> and mode="primary".

    4. Localization:
        	Use the Odoo translation system (_()) to make email templates multilingual.

Email templates are often integrated with actions, such as a "Send by Email" button, and triggered by server actions or user workflows.

### **Server Action**

Server actions are used to execute custom logic, trigger automation, or call specific methods on records based on certain conditions or user interactions.They are also defined within the **data** directory of an module. These templates are defined using the < record > tag with the model **ir.actions.server**.

**Key Fields in Server Action Definitions**

Basic Fields:

   1. name:
        The name of the server action.
        Example:
			<field name="name">Approve Order</field>

   2. model_id:
        Specifies the model on which the action operates.
        Example: 
			<field name="model_id" ref="sale.model_sale_order"/>

   3. state:
        Specifies the action type. Possible values:
         - code: Executes Python code.
         -    object: Calls a method on the model.
         -   multi: Calls a method for multiple records.
         -    email: Sends an email.
         -   url: Redirects to a URL.
        Example:
					<field name="state">code</field>

   4. code:
      Contains the Python code to be executed if the state is set to code.
        Example:

			<field name="code">
    		if record.state == 'draft':
        		record.action_confirm()
			</field>

**Example Server Action Definitions**
1. A Server Action with Python Code

	Example:
This server action validates a sale order when triggered.
		<odoo>
    		<data>
        		<record id="server_action_validate_order" model="ir.actions.server">
            		<field name="name">Validate Order</field>
            		<field name="model_id" ref="sale.model_sale_order"/>
            		<field name="state">code</field>
            		<field name="code">
                			if record.state == 'draft':
                    		record.action_confirm()
            		</field>
        		</record>
    		</data>
		</odoo>

2. Automatic Trigger on Record Creation

	Example:
This server action automatically assigns the current user to new sale orders when they are created.
		<odoo>
    		<data>
        		<record id="server_action_assign_user" model="ir.actions.server">
            		<field name="name">Assign User to Sale Order</field>
            		<field name="model_id" ref="sale.model_sale_order"/>
            		<field name="binding_model_id" ref="sale.model_sale_order"/>
            		<field name="binding_type">on_create</field>
            		<field name="state">code</field>
            		<field name="code">
                		record.write({'user_id': user.id})
            		</field>
        		</record>
    		</data>
		</odoo>

3. Automatic Trigger on Record Creation

	Example:
This server action triggers when a field value in the record changes, e.g., updating the state of an invoice.
		<odoo>
    		<data>
        		<record id="server_action_on_update" model="ir.actions.server">
            		<field name="name">Notify on Invoice State Change</field>
            		<field name="model_id" ref="account.model_account_move"/>
            		<field name="binding_model_id" ref="account.model_account_move"/>
            		<field name="binding_type">on_write</field>
            		<field name="state">code</field>
            		<field name="code">
                			if record.state == 'posted':
                    		record.message_post(body="The invoice has been posted.")
            		</field>
        		</record>
    		</data>
		</odoo>


**Server Actions Workflow in Odoo
**
   1. Manual Execution:
        Server actions can be added to the "Action" menu in Odoo views, allowing users to execute them manually.

   2. Automatic Execution:
        Define binding_model_id and binding_type to make the server action trigger automatically based on a record's lifecycle (e.g., creation, update, or deletion).

   3. Integration with Automated Actions:
        Server actions can be combined with automated actions (from the "Settings" menu) to streamline workflows.

   4. Security:
        Ensure your Python code includes necessary checks to prevent unintended behavior or security risks.


### **Sequences**
Used to generate unique identifiers for various models, they are also defined within the **data** directory in an XML file using the model **ir.sequence**.

**Key Components of Sequence Definition**

   1. name:
        The name of the sequence.
        Example:
			<field name="name">Invoice Sequence</field>

   2. code:
        A unique technical identifier for the sequence, used programmatically.
        Example:
			<field name="code">account.move</field>

   3. prefix:
        A prefix for the sequence value.
        Example:
			<field name="prefix">INV/</field>

   4. padding:
        Determines the number of digits in the sequence number.
        Example:
			<field name="padding">5</field>

   5. number_next:
        The next number in the sequence.
        Example: 
			<field name="number_next">1</field>

   6. number_increment:
        The step by which the sequence increases.
        Example:
			<field name="number_increment">1</field>

   7. implementation:
        Defines whether the sequence is implemented in standard or no_gap mode.
        Example:
			<field name="implementation">standard</field>

   8. company_id:
        Restricts the sequence to a specific company.
        Example: 
			<field name="company_id" ref="base.main_company"/>

   9. use_date_range:
        If set to True, generates a new sequence for each date range (e.g., each year).
        Example: 
			<field name="use_date_range">True</field>


**Example of Sequence Definition**

	<odoo>
    	<data>
        	<record id="sequence_invoice" model="ir.sequence">
            	<field name="name">Invoice Sequence</field>
            	<field name="code">account.move</field>
            	<field name="prefix">INV/%(year)s/</field>
            	<field name="padding">5</field>
            	<field name="number_next">1</field>
            	<field name="number_increment">1</field>
            	<field name="implementation">standard</field>
            	<field name="use_date_range">True</field>
        	</record>
    	</data>
	</odoo>


**Using Sequences in Code**
   1. Calling a Sequence in Python: Use the env['ir.sequence'].next_by_code() method to fetch the next value for a sequence:

   	sequence = self.env['ir.sequence'].next_by_code('account.move')

   2. Linking Sequences to Models: Define the sequence for a specific model by setting it in the create method or field default:

   		class SaleOrder(models.Model):
    			_inherit = 'sale.order'

    			name = fields.Char(default=lambda self: self.env['ir.sequence'].next_by_code('sale.order'))


**Best Practices for Sequences**

   1. Use Unique Codes: Ensure the code for each sequence is unique to avoid conflicts.

   2. Padding and Prefix:
        Choose padding and prefixes that are meaningful and comply with business/legal requirements.
        **Example**: Use INV/%(year)s/ for invoices to include the year.

   3. Date Ranges:
        Enable use_date_range for sequences that require yearly resets (e.g., invoice numbers).

   4. No-Gap Sequences:
        Use no_gap mode only if required by law or strict compliance rules, as it may impact performance.


### **Scheduled tasks (Cron Jobs)**

These tasks allow you to automate actions that should be executed at specific intervals, such as daily reports, data synchronization, or periodic cleanup operations. These are also  defined within the **data** directory in an XML file using the model **ir.cron.**


**Key Fields in Cron Job Definitions**

Basic Fields

   1. name:
        The name of the cron job.
        Example: 
			<field name="name">Process Pending Orders</field>

   2. model_id:
        The model on which the cron job operates.
        Example:
			<field name="model_id" ref="sale.model_sale_order"/>

   3. state:
        Specifies the action type. Use code to execute Python code or call a specific model method.
        Example:
			<field name="state">code</field>

   4. code:
        Contains the Python code to execute.
        Example:
    		<field name="code">model.process_pending_orders()</field>

   5. active:

    Determines whether the cron job is active or not.
    Example: 
			<field name="active">True</field>

   6. interval_type:

    Specifies the time unit for the interval. Possible values:
      -   minutes
      -   hours
      -   days
      -   weeks
      -   months
    Example: 
				<field name="interval_type">hours</field>

   7. interval_number:

    The frequency of execution, combined with interval_type.
    Example: 
			<field name="interval_number">1</field> (runs every 1 hour).

   8. nextcall:

    The date and time for the next execution of the task.
    Example:
			<field name="nextcall">2024-01-01 00:00:00</field>

   9. user_id:

    Specifies the user context under which the task will run.
    Example: 
			<field name="user_id" ref="base.user_root"/>


**Example Cron Job Definitions**

	<odoo>
    	<data>
        	<record id="cron_send_reminders" model="ir.cron">
            	<field name="name">Send Customer Reminders</field>
            	<field name="model_id" ref="mail.model_mail_message"/>
            	<field name="state">code</field>
            	<field name="code">model.send_reminders()</field>
            	<field name="active">True</field>
            	<field name="interval_number">1</field>
            	<field name="interval_type">days</field>
            	<field name="nextcall">2024-01-01 08:00:00</field>
            	<field name="user_id" ref="base.user_root"/>
        	</record>
    	</data>
	</odoo>

**Executing Cron Jobs in Python**

   1. Define the Method in the Model: Implement the logic for the cron job in the corresponding model. For example:
  
			from odoo import models, api

	  	 class SaleOrder(models.Model):
    			_inherit = 'sale.order'

    		@api.model
    			def process_pending_orders(self):
        			pending_orders = self.search([('state', '=', 'draft')])
        			for order in pending_orders:
            			order.action_confirm()

   2. Link the Method to the Cron Job: Use the code field in the XML to call this method:
   
   		<field name="code">model.process_pending_orders()</field>

**Key Points to Remember**

   1. Time Zone Handling:
        The nextcall field is in UTC, so adjust it based on your time zone.

   2. Deactivating Cron Jobs:
        To disable a cron job, set < field name="active">False< /field>.

   3. Security Context:
        The user_id determines the user context under which the task is executed. Use base.user_root (Administrator) for tasks requiring full access.

   4. Performance Considerations:
        Avoid running resource-intensive tasks too frequently. Use appropriate intervals (interval_number and interval_type).

   5. Error Handling:
        Include proper exception handling in Python methods to avoid breaking the cron job execution.
