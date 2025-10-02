# Concept

<div dir="" align="center">
<img src="https://gitlab.marger.it:10443/ruhtra/compo/-/wikis/Compo_explication.png" >
</div>

A **Component** is a block that *encapsulates technical code*.

An **Interface** is a list of functions and data. You can see it as the **API** of a component:

* If a component *implements* an interface, it will appear in the **PROVIDE** list.
* If a component *requires* an interface to work, it will appear in the **REQUIRE** list.
  
A **LINK** is a RPC communication channel that transports **function Call on interface**:

* If a component wants to **provide** function, the link must expose them.
* If a component wants to **require** a function, the link need to provide a connection to a external link.

A **Bus** is a unidirectional communication channel that transports **Events**:

* If a component wants to **receive** events, the bus must be added to its **RECEIVER** list.
* If a component wants to **emit** events, the bus must be added to its **EMITTER** list.

**Data** represents the internal state or attributes of a component.

A **SubComponent** is a component nested inside another component, allowing hierarchical composition.
