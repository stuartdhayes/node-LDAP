node-ldap
=========

OpenLDAP client bindings for Node.js. A good start, but not, as of
this writing, production quality. The API will change slightly over
the next week or two.

Contributing
------------

This module works, but is not yet ready for production. It is being
made available at this early stage to funnel contributors who may
otherwise begin their own module in parallel. Any and all patches are
certainly welcome.

Dependencies
------------

Tested with Node v0.2.3.

To build, ensure the OpenLDAP client libraries are installed, and

   node-waf configure build

Search Example
--------------

        var LDAP = require("LDAP");
        var lconn = new LDAP.Connection();
        
        // Open a connection.
        lconn.Open("ldap://ldap1.example.com,ldap://ldap2.example.com",
            function() {
                lconn.Search("o=company", "(uid=alice)", "*", function(res) {
                console.log(res[0].uid[0]);
             });
        });

Authenticate Example
--------------------

        var LDAP = require("LDAP");
        var lconn = new LDAP.Connection();

        // Open a connection. 
        lconn.Open("ldap://ldap1.example.com,ldap://ldap2.example.com", 
            function() {
                lconn.Authenticate("cn=alice,o=company", "seCretStuff", function(res) {
                    // authenticated. Try a search.
                    lconn.Search("o=company", "(uid=bob)", "*", function(res) {
                        console.log("Bob has a cn of "+res[0].cn[0]);
                    });                                        
                });
             });


TODO:
-----

* On disconnect, the library starts a reconnect attempt immediately. I
  should likely wait until the next query comes along
* However, if there are in-flight queries, it should reconnect immediately.
* reconnect handling in general is a little flaky
* individual query timeout handling
* proper packaging required, with package.json and all that goodness.
* Support update methods as well as search and authenticate.
