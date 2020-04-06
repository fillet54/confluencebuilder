.. -*- restructuredtext -*-

=======================================
Atlassian Confluence Builder for Sphinx
=======================================

.. image:: https://img.shields.io/pypi/v/sphinxcontrib-confluencebuilder.svg
   :target: https://pypi.python.org/pypi/sphinxcontrib-confluencebuilder
   :alt: pip Version

.. image:: https://travis-ci.com/sphinx-contrib/confluencebuilder.svg?branch=master
   :target: https://travis-ci.com/sphinx-contrib/confluencebuilder
   :alt: Build Status

.. image:: https://readthedocs.org/projects/sphinxcontrib-confluencebuilder/badge/?version=latest
   :target: https://sphinxcontrib-confluencebuilder.readthedocs.io/en/latest/?badge=latest
   :alt: Documentation Status

.. image:: https://img.shields.io/pypi/dm/sphinxcontrib-confluencebuilder.svg
   :target: https://pypi.python.org/pypi/sphinxcontrib-confluencebuilder/
   :alt: PyPI download month

Sphinx_ extension to build Confluence storage format files and optionally
publish them to a Confluence instance.

Requirements
============

* Python_ 2.7 or 3.5+
* Requests_ 2.14.0+
* Sphinx_ 1.8+

If publishing:

* Confluence_ Cloud or Server 6.7+

Installing
==========

The recommended method to installing this extension is using pip_:

.. code-block:: shell

   pip install sphinxcontrib-confluencebuilder
    (or)
   python -m pip install sphinxcontrib-confluencebuilder

For a more in-depth installation information, see also:

 | Atlassian Confluence Builder for Sphinx - Installation
 | https://sphinxcontrib-confluencebuilder.readthedocs.io/en/stable/install.html

Usage
=====

- Register the extension ``sphinxcontrib.confluencebuilder`` in the project's
  configuration script (``conf.py``):

.. code-block:: python

   extensions = ['sphinxcontrib.confluencebuilder']

- Run sphinx-build with the builder ``confluence``:

.. code-block:: shell

   sphinx-build -b confluence . _build/confluence -E -a
    (or)
   python -m sphinx -b confluence . _build/confluence -E -a

For more information on the usage of this extension, see also:

 | Atlassian Confluence Builder for Sphinx - Tutorial
 | https://sphinxcontrib-confluencebuilder.readthedocs.io/en/stable/tutorial.html

Configuration
=============

The following is an example of a simple configuration for Confluence generation
and publishing:

.. code-block:: python

   extensions = ['sphinxcontrib.confluencebuilder']
   confluence_publish = True
   confluence_space_name = 'TEST'
   confluence_parent_page = 'Documentation'
   confluence_server_url = 'https://intranet-wiki.example.com/'
   confluence_server_user = 'username'
   confluence_server_pass = 'api-key-or-password'

For a complete list of configuration options, see also:

 | Atlassian Confluence Builder for Sphinx - Configuration
 | https://sphinxcontrib-confluencebuilder.readthedocs.io/en/stable/configuration.html

Directives
==========

For a complete list of directives supported by this extension, please consult:

 | Atlassian Confluence Builder for Sphinx - Directives
 | https://sphinxcontrib-confluencebuilder.readthedocs.io/en/stable/directives.html

Demonstration
=============

The set of example documents used to assist in validation/testing can be found
here:

 | Atlassian Confluence Builder for Sphinx - Validation Set
 | https://github.com/sphinx-contrib/confluencebuilder/tree/master/test/validation-sets

The active and older versions of published validation documents can be found
here:

 | Atlassian Confluence Builder for Sphinx - Online Demo on Confluence Cloud
 | https://jdknight.atlassian.net/wiki/spaces/confluencebuilder/

Supported Markup
================

For a complete list of supported markup, consult the following:

 | Atlassian Confluence Builder for Sphinx - Markup
 | https://sphinxcontrib-confluencebuilder.readthedocs.io/en/stable/markup.html

Sphinx Tabs Support
===================
Using sphinx-tabs requires the following two user-macros to be installed:

.. important::
   The **Macro Name** must match exactly as listed below

+-----------------------+-------------------------------+
| Field                 | Value                         |
+=======================+===============================+
| Macro Name            | tab-container                 |
+-----------------------+-------------------------------+
| Visibility            | Visible to all                |
+-----------------------+-------------------------------+
| Macro Title           | Tab Container                 |
+-----------------------+-------------------------------+
| Description           | Container which holds tabs    |
+-----------------------+-------------------------------+
| Categories            | Formatting                    |
+-----------------------+-------------------------------+
| Macro Body Processing | Rendered                      |
+-----------------------+-------------------------------+
| Template              | See code-block below          |
+-----------------------+-------------------------------+

.. code-block:: javascript

   ## Macro title: Tab Container
   ## Macro has a body: Y
   ## Body processing: Selected body processing option
   ## Output: Selected output option
   ##
   ## Developed by: Phillip Gomez
   ## Date created: 04/04/2020
   ## Installed by: Phillip Gomez

   ## @noparams

   <style>
   .tabs {
     position: relative;   
     min-height: 200px; /* This part sucks */
     clear: both;
     overflow: hidden;
   }
   .tab {
     float: left;
     margin-top: 15px;
   }
   .tab label {
     background: #eee; 
     padding: 10px; 
     border: 1px solid #ccc; 
     margin-left: -1px; 
     position: relative;
     left: 1px; 
     border-top-left-radius: 5px;
     border-top-right-radius: 5px;
   }
   .tab [type=radio] {
     display: none;   
   }
   .content {
     position: absolute;
     top: 43px;
     left: 0;
     background: white;
     right: 0;
     bottom: 0;
     padding: 20px;
     border: 1px solid #ccc; 
   }
   [type=radio]:checked ~ label {
     background: white;
     border-bottom: 1px solid white;
     z-index: 2;
   }
   [type=radio]:checked ~ label ~ .content {
     z-index: 1;
   }
   </style>

   <div class="tabs">
      $body
   </div>
   <script type="text/javascript">

   if (typeof window.confluence_tabs === 'undefined') {
       window.confluence_tabs = {

           groupId: 1,
           MENU_HEIGHT: 56,
           PADDING: 14,

           setTabContainerHeight: function(current, count) {
               var tab = current,
                   tabGroup = tab.parentElement,
                   input = tab.children[0],
                   innerContent = tab.children[2].children[0];

               if ( !input.checked )
                   return;

               if ( window.confluence_tabs.imagesLoaded(tab) ) {
                   var otherHeight = window.confluence_tabs.MENU_HEIGHT + 2*window.confluence_tabs.PADDING;
                   tabGroup.style.minHeight = "" + (otherHeight + innerContent.offsetHeight) + "px";
               }
               else {
                   var timeout = count < 10 ? 50 : 500;
                   setTimeout( function() { 
                       window.confluence_tabs.setTabContainerHeight(tab, count+1);
                   }, timeout);
               }
           },

           imagesLoaded: function (parent) {
               var allImages = parent.getElementsByTagName("img"),
                   complete = true;

               for (var i = 0; i < allImages.length; i++) {
                   complete = complete && allImages[i].complete;
                   if (!complete)
                       break;
               }
               return complete;
           },

           getTabGroup: function() {
               // since this script is inline this can assume we are last in
               // the list of scripts

               var scripts = document.getElementsByTagName('script'),
                   currentScript = scripts[scripts.length - 1],
                   tabGroup;

               while (true) {
                   tabGroup = currentScript.previousSibling;

                   // Sometimes text gets in between
                   if (tabGroup.classList && tabGroup.classList.contains("tabs"))
                       break;

                   currentScript = tabGroup;
               }
               return tabGroup;
           },

           initTabs: function() {
               var tabGroup = window.confluence_tabs.getTabGroup(),
                   tabGroupId = window.confluence_tabs.groupId;
                   tabs = tabGroup.children;

               // Increment group ID for next macro use on page
               window.confluence_tabs.groupId++;

               for (var i = 0; i < tabs.length; i++) {
                   var tab = tabs[i],
                       tabInput = tab.children[0],
                       tabLabel = tab.children[1],
                       id = "tab-group-" + tabGroupId + "-tab-" + (i + 1);

                   tabInput.id = id;
                   tabInput.name = "tag-group-" + tabGroupId;
                   tabLabel.setAttribute("for", id);

                   // select first tab
                   if (i == 0)
                       tabInput.checked = true;

                   tabInput.onchange = function(evt) {
                       if ( evt.target.checked )
                           window.confluence_tabs.setTabContainerHeight(evt.target.parentElement, 0)
                   }
               }
           }
       };
   }

   window.confluence_tabs.initTabs();

   window.confluence_tabs.setTabContainerHeight(window.confluence_tabs.getTabGroup().children[0], 0);

   </script>


+-----------------------+-------------------------------+
| Field                 | Value                         |
+=======================+===============================+
| Macro Name            | tab-item                      |
+-----------------------+-------------------------------+
| Visibility            | Visible to all                |
+-----------------------+-------------------------------+
| Macro Title           | Tab Item                      |
+-----------------------+-------------------------------+
| Description           | Individual Tab                |
+-----------------------+-------------------------------+
| Categories            | Formatting                    |
+-----------------------+-------------------------------+
| Macro Body Processing | Rendered                      |
+-----------------------+-------------------------------+
| Template              | See code-block below          |
+-----------------------+-------------------------------+

.. code-block:: javascript

   ## Macro title: Tab Item
   ## Macro has a body: Y
   ## Body processing: Selected body processing option
   ## Output: Selected output option
   ##
   ## Developed by: Phillip Gomez
   ## Date created: 04/04/2020
   ## Installed by: Phillip

   ## @param Title:title=Title|type=string|required=true|desc=Tab Title

   <div class="tab">
          <input type="radio">
          <label>$webwork.htmlEncode($paramTitle)</label>

          <div class="content">
              <div>
              $body
              </div>
          </div> 
   </div>


.. _Confluence: https://www.atlassian.com/software/confluence
.. _Python: https://www.python.org/
.. _Requests: https://pypi.python.org/pypi/requests
.. _Sphinx: https://www.sphinx-doc.org/
.. _pip: https://pip.pypa.io/
