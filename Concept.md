
# Concept

<div dir="" align="center">
<img src="https://gitlab.marger.it:10443/ruhtra/compo/-/wikis/Compo_explication.png" >
</div>

The Component is a block that "Encapsulate Technical Code".

A Interface is a list of function and data. You can see it a the API of a component.
If a Compoment implement a interface you will find it in the PROVIDE list.
If a Compoment need a interface to work you find it in the REQUIRE list.

A Bus is a comunication chanel that go in only one direction. The element that move to this chanel is "Event".
If a Compoment want to receive event, they have to add the bus to the RECIEVER list.
If a Compoment want to emit some event they have to add the bus to the EMITER list.

A Data is internal data that define the component.
A SubComponent is a component that is a component but inside a component.
