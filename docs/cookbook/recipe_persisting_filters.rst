Persisting Filters
==================

Persisting filters allow your application to save the filters the authenticated
user has submitted.
Then the saved filters will be reused if the page is displayed again.


Enable Filters Persistence
--------------------------

By default, filters persistence is disabled.
You can enable it in your ``sonata_admin`` configuration :

.. configuration-block::

    .. code-block:: yaml

        # app/config/config.yml

        sonata_admin:
            persist_filters: true


Choose the persistence strategy
-------------------------------

When you enable the filters persistence by setting ``persist_filters``
to ``true``.
SonataAdmin will use the default filter persister :
``Sonata\AdminBundle\Filter\Persister\SessionFilterPersister``
(which is, by now, the only one provided).

You can implement your own filter persister by creating a new class that
implements the ``Sonata\AdminBundle\Filter\Persister\FilterPersisterInterface``
interface and registering it as a service.
Then the only thing to do is to tell SonataAdmin to use this service as
filter persister.


Globally :

.. configuration-block::

    .. code-block:: yaml

        # app/config/config.yml

        sonata_admin:
            persist_filters: true
            filter_persister: filter_persister_service_id


Per Admin :

.. configuration-block::

    .. code-block:: xml

        <!-- src/AppBundle/Resources/config/admin.xml -->

        <service id="app.admin.user" class="AppBundle\Admin\UserAdmin">
            <tag name="sonata.admin" manager_type="orm" filter_persister="filter_persister_service_id" />
            <argument />
            <argument>AppBundle\Entity\User</argument>
            <argument />
        </service>

    .. code-block:: yaml

        # src/AppBundle/Resources/config/admin.yml

        services:
            app.admin.user:
                class: AppBundle\Admin\UserAdmin
                tags:
                    -
                        name: sonata.admin
                        manager_type: orm
                        filter_persister: filter_persister_service_id
                arguments:
                    - ~
                    - AppBundle\Entity\User
                    - ~


Disable filters persistence for some Admin
------------------------------------------

When you enable the filters persistence by setting ``persist_filters``
to ``true``.
All registered Admins will have the feature enabled.

You can disable it per Admin if you want.


.. configuration-block::

    .. code-block:: xml

        <!-- src/AppBundle/Resources/config/admin.xml -->

        <service id="app.admin.user" class="AppBundle\Admin\UserAdmin">
            <tag name="sonata.admin" manager_type="orm" persist_filters="false" />
            <argument />
            <argument>AppBundle\Entity\User</argument>
            <argument />
        </service>

    .. code-block:: yaml

        # src/AppBundle/Resources/config/admin.yml

        services:
            app.admin.user:
                class: AppBundle\Admin\UserAdmin
                tags:
                    -
                        name: sonata.admin
                        manager_type: orm
                        persist_filters: false
                arguments:
                    - ~
                    - AppBundle\Entity\User
                    - ~


.. note::

    Both ``persist_filters`` and ``filter_persister`` can be used globally
    and per-admin, which provide you the most flexible way to configure
    this feature.

