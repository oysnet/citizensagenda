Types de documents
============================
	
Il existe quatres types de documents: :ref:`event <doc_event>`, :ref:`user <doc_user>`, :ref:`group <doc_group>` et :ref:`schema <doc_schema>`.

Les documents de type *user* contiennent les informations relatives aux utilisateurs du service.

Les documents de type *schema* sont des documents particuliers utilisés pour décrire et valider le contenu des documents de type *event*


Attributs communs à tous les documents
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ 
	
.. warning:: Les attributs ci-dessous sont en lecture seule. 
	
.. _common_id:
	
``_id (String)``  
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Identifiant unique au travers de tous les documents du service, quelques soient leurs types.

	
``_rev (String)``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Identifiant de révision. 

	
``type (String)``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Le type de document: event, user ou schema.

``hash (String)``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Son mode de calcul est spécifique à chaque type de document. Cet attribut permet d'empecher la création de documents en double.  


.. _doc_user:

Document de type *user*
^^^^^^^^^^^^^^^^^^^^^^^
	
Ces documents contiennent les données relatives aux comptes utilisateurs.
	
``first_name (String)``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	
	Prénom de l'utilisateur
	
``last_name (String)``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	
	Nom de l'utilisateur
		
``email (String)``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	
	Adresse email de l'utilisateur.
	

``password (String)``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	
	Mot de passe crypté de l'utilisateur.

	
``is_active (Boolean)``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	
	Booléen indiquant si l'utilisateur est actif. Les utilisateurs inactifs ne peuvent pas accèder au service.
	
	
``is_staff (Boolean)``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	
	Booléen indiquant si l'utilisateur est membre de l'équipe d'administration. Seule l'équipe d'administration a accès à l'outil d'administration.
	
	L'accès aux fonctionnalités de l'outils d'administration est affiné via une gestion de groupe.

	
``is_superuser (Boolean)``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	
	Booléen indiquant si l'utilisateur est super utilisateur. Un super utilisateur a accès sans restriction à toutes les fonctionnalités du service.
	

``last_login (Datetime)``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	
	Date de la dernière authentification sur le service.
	
``date_joined (Datetime)``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	
	Date d'inscription au service.
		
.. _doc_event:

Document de type *event*
^^^^^^^^^^^^^^^^^^^^^^^^^

.. _doc_event_event:

``event (Object)``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

L'attribut *event* contient une structure de données décrivant l'évènement. Cette structure est contrainte par un document de type :ref:`schema <doc_schema>`.


``author (String)``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Contient l'identifiant (attribut :ref:`_id <common_id>`) du document de type *user*

``status (Enum)``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Définit le status de 'évènement. Les valeur possible sont:
	* canceled : annulé
	* confirmed : confirmé
	* tentative : proposition

``write_groups (List)``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Contient les identifiant des documents de type :ref:`group <doc_group>` ayant accès en écriture sur le document.

**Si la longueur du tableau est 0, aucune restriction n'est appliquée.**

``read_groups (List)``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Contient les identifiant des documents de type :ref:`group <doc_group>` ayant accès en lecture sur le document.

Si la longueur du tableau est 0, aucune restriction n'est appliquée.

``write_users (List)``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Contient les identifiant des documents de type :ref:`user <doc_user>` ayant accès en écriture sur le document.

Si la longueur du tableau est 0, aucune restriction n'est appliquée.

``read_users (List)``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Contient les identifiant des documents de type :ref:`user <doc_user>` ayant accès en lecture sur le document.

Si la longueur du tableau est 0, aucune restriction n'est appliquée.
 

.. _doc_schema:

Document de type *schema*
^^^^^^^^^^^^^^^^^^^^^^^^^

Les documents de type *schema* sont des documents particuliers utiliser pour décrire et valider le contenu de l'attribut :ref:`event <doc_event_event>` des documents de type :ref:`event <doc_event>`. 

Comme les autres types de document, ce type de document contient des attributs systèmes ainsi que des attributs qui lui sont propres. Parmis ces attributs, l'attribut :ref:`schema <doc_schema_schema>` contient les données utilisées pour la validation.

.. _doc_schema_schema:

``schema (Object)``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Contient les données de validation. Il s'agit d'une structure d'objet définie par les spécifications `json-schema-03 <http://tools.ietf.org/html/draft-zyp-json-schema-03>`_

``final (Boolean)``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Définit si un schéma peut etre utilisé pour valider un évènement, si final vaut false le schéma peut être utilisé dans un héritage de schéma.

``sample (Object)``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Contient un exemple de structure de données validant le schéma.

``template (String)``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Contient un template pouvant être utilisé pour obtenir une structure html simple d'un évènement utilisant ce schema

``status (Enum)``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Définit le status du document:
	* PUBLISHED : le document est utilisable
	* DRAFT : le document est en cours de création, il n'est pas utilisable
	* DEPRECATED : le document est utilisé, mais son utilisation pour valider de nouveau èvènements est déconseillée


.. _doc_group:

Document de type *group*
^^^^^^^^^^^^^^^^^^^^^^^^^	