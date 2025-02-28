Attachments on Swift storage
============================

This addon enable storing attachments (documents and assets) on OpenStack Object Storage (Swift)

Configuration
-------------

Activate Swift storage:

* Create or set the system parameter with the key ``ir_attachment.location`` with the following value ``swift``.

Configure accesses with environment variables:

* ``SWIFT_AUTH_URL``            : URL of the Swift server
* ``SWIFT_TENANT_NAME``
* ``SWIFT_ACCOUNT``
* ``SWIFT_PASSWORD``
* ``SWIFT_REGION_NAME``         : optional region
* ``SWIFT_WRITE_CONTAINER``     : Name of the container to use in the store (created if not existing)

Read-only mode:

The container name and the key are stored in the attachment. So if you change the
``SWIFT_WRITE_CONTAINER`` or the ``ir_attachment.location``, the existing attachments
will still be read on their former container. But as soon as they are written over
or new attachments are created, they will be created on the new container or on
the other location (db or filesystem). This is a convenient way to be able to
read the production attachments on a replication (since you have the
credentials) without any risk to alter the production data.

This addon must be added in the server wide addons with (``--load`` option):

``--load=web,web_kanban,attachment_swift``

Python Dependencies
-------------------

This module needs the python-swiftclient and the python-keystoneclient (For auth v2.0) to work.
The python-keystoneclient needs the linux package build-essential and python-dev to install properly.

The python-swiftclient can be used from the command line, useful to test:

    export AUTH_VERSION=2.0
    export OS_USERNAME={SWIFT_ACCOUNT}
    export OS_PASSWORD={SWIFT_PASSWORD}
    export OS_TENANT_NAME={SWIFT_TENANT_NAME}
    export SWIFT_REGION_NAME={SWIFT_REGION_NAME}
    export OS_AUTH_URL=https://auth.cloud.ovh.net/v2.0
    swift stat

More information at
https://docs.openstack.org/python-swiftclient/latest/cli/index.html#swift-usage
