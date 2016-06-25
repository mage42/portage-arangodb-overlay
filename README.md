portage-arangodb-overlay
========================

Gentoo Portage overlay for ArangoDB
-----------------------------------

[ArangoDB - the multi-model NoSQL database](https://www.arangodb.com/).

/etc/layman/layman.cfg add:

    overlays: ...
              https://github.com/gbevan/portage-arangodb-overlay/raw/master/repository.xml

run:

    layman -f -a gbevan-arangodb
    emerge -v --ask arangodb

to start (systemd):

    systemctl enable arangodb
    systemctl start arangodb

(pre-systemd init scripts are also provided - let me know if you have any issues/fixes)

Upgrading
---------

The ebuild for v3 is not yet working...

If upgrading from v2.* to v3.* - read the ArangoDB docs.  The database format has changed to use VelocityPack, and is
not compatible with the 2.* version database.  See:

https://docs.arangodb.com/3.0/Manual/Administration/Upgrading/Upgrading30.html

    layman -s gbevan-arangodb
    emerge -v -u arangodb

Ensure all active connections, from applications and arangosh etc, are shutdown/closed before restarting:

    systemctl daemon-reload
    systemctl restart arangodb

Check the logs:

    journalctl -b -u arangodb

If you get message like:

    FATAL Database 'partout' needs upgrade. Please start the server with the --upgrade option

then:

    arangod --uid arangodb --gid arangodb --upgrade
    (there may be an upgrade option via systemctl... maybe...)


Developer Notes:
----------------

* When releasing new ebuild, run:

        ebuild arangodb-3.x.x.ebuild manifest

  for your new ebuild version, then commit/push.
