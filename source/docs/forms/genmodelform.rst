.. _class-genmodelform:

GenModelForm
============

.. code:: python

    from codenerix.forms import GenModelForm


    class ModelNameForm(GenModelForm):

        class Meta:
            model = ModelName
            exclude = []

        def __groups__(self):
            return [
                (
                    _(u'groupName'), 12, 
                    ['attr1', 6], 
                    ['attr2', 6], 
                    ['attr3', 6],
                )
            ]

        @staticmethod
        def __groups_details__():
            return [
                (
                    _(u'groupName'), 12, 
                    ['attr1', 6], 
                    ['attr2', 6], 
                    ['attr3', 6],
                )
            ]



Description
+++++++++++

GenModelForm is a generic class of Codenerix used to define a form created from a model. This class inherits from django class forms.ModelForm
We recomended that the name of each class which inherit from GenModelForm follow this pattern: NameModel+Form.

- Example:
    If model is User, then, GenCreate declarations should be, ``class UserForm(GenModelForm):`` 

Attribute
+++++++++

**Warning**
GenModelForm inherits from forms.ModelForm, so attributes declaration is a little different than other classes. In GenModelForm all attributes are put into ``class Meta:``

========
autofill
========

Autofill is an attribute designed to fill foreignkey attributes with data based in a textbox, other information of the form or an AngularJS function. The structure is a dictionary with the attribute name as key and a list as value. This list consists in the following ordered fields:

-  field[0]: Type of the selector that you need to create:
   
   -  **select** : Show a box that take a single choice.
   -  **multiselect** : Show a box which let you select multiple choices.

-  field[1]: Minimum number of letters required in the textbox to start the execution of a search. (If you want to show all records in the textbox you should write '*')

-  field[2]: Related name of the :ref:`class-genforeignkey` class used to fetch the field data.  

-  field[3-n]: (``Optionals fields``) The rest of fields are filters used to send the pk of a the selected attribute. Usually is used to take the value of an attribute and perform the search using it. There is two options to use this filters:
   
   -  attrName: Take actual value, if exist, from a selected attribute.
   -  __pk__: Send pk of own object.    

.. code:: python

    autofill = {
        # Search of attr3 based in attr2
        'attr3': ['select', 3, related_name_foreignkey_url,'attr2'],
        # Search based in his own pk.
        'attr5': ['select', 3, related_name_foreignkey_url,'__pk__'],
        # Search based in his own pk and attr2.
        'attr4': ['select', 3, related_name_foreignkey_url,'__pk__', 'attr2'],
    }

=======
exclude
=======

Used to exclude attributes from the model that shouldn't be shown in the view. **Available in django**

=====
model
=====

Model used to generate the form. **Available in django**


Method
++++++

================
__groups__(self)
================

This method is used to define the order and layout of the form fields. Its goal is to be used as an styling tool for the forms.

.. code:: python

    def __groups__(self):
        groups = [
            (
                _('Group name'), 12,
                ['attr1', 6, '#23fe33', '#005476', 'left', True, _('Alternative label')],
                ['attr2', 6, None, None, True], 
                ['attr3', 3],
                ['attr4', 12, None, None, None, None, None, ['ng-change=function()']],
            )
        ]  
        return groups

Return
------

Groups return a list of tuples. Each tuple represent in template a different box. Each tuple have next structure:

- Name of the box.
- Number of columns which the box takes. ``Remember: max númber of columns it's 12``
- From position 2 to the last are a list of options which control how to render each declared field. The structure of the list is:
   - attr[0]: Name of field (Named or not in the model).
   - attr[1]: Number of columns which takes this field.
   - attr[2]: Label color (HTML color code).
   - attr[3]: Padding color (HTML color code).
   - attr[4]: Alignment of the label. Options are (left|center|right)
   - attr[5]: Label and field are in the same line? (Boolean). **Is used for checkbox and roundcheck**
   - attr[6]: Alternative label. If there is some text specified Codenerix will show it on the template (Default value is None).
   - attr[7]: Extra data. A list of extra data that is sent to template. (Is sended in plain text)


More examples
+++++++++++++

==================
Basic GenModelForm
==================

.. code:: python

    from codenerix.forms import GenModelForm


    class ModelNameForm(GenModelForm):

        class Meta:
            model = ModelName
            exclude = []

        def __groups__(self):
            return [
                (
                    _(u'groupName'), 12, 
                    ['attr1', 6], 
                    ['attr2', 6], 
                    ['attr3', 6],
                )
            ]

        @staticmethod
        def __groups_details__():
            return [
                (
                    _(u'groupName'), 12, 
                    ['attr1', 6], 
                    ['attr2', 6], 
                    ['attr3', 6],
                )
            ]


============================
GenModelForm with foreignkey
============================

.. code:: python

    from codenerix.forms import GenModelForm


    class ModelNameForm(GenModelForm):

        class Meta:
            model = ModelName
            exclude = ['attr5']
            # attr3 it's a foreignkey field, and start to search with 3 characters with condition epecified in view of related name.
            autofill = {
                'attr3': ['select', 3, related_name_foreignkey_url, 'attr2'],
            }

        def __groups__(self):
            return [
                (
                    _(u'groupName'), 12,
                    ['attr1', 6, '#23fe33', '#005476', 'left', True, _('Alternative label')],
                    ['attr2', 6, None, None, True], 
                    ['attr3', 3],
                    ['attr4', 12, None, None, None, None, None, ['ng-change=function()']],
                )
            ]

        @staticmethod
        def __groups_details__():
            return [
                (
                    _(u'nameGroup'), 12, 
                    ['attr1', 6],
                    ['attr2', 6],
                    ['attr3', 6],
                )
            ]
