.. _advanced-custom-attribute:

``RequestContext`` custom attributes
====================================

When you are using multiple decorators, you might want to pass some value to the next decorator.
You can do this by attaching attributes to a :api:`RequestContext`. To attach an attribute,
you need to define an ``AttributeKey`` first:

.. code-block:: java

    import io.netty.util.AttributeKey;

    public final class MyAttributeKeys {
        public static final AttributeKey<Integer> INT_ATTR =
                AttributeKey.valueOf(MyAttributeKeys.class, "INT_ATTR");
        public static final AttributeKey<MyBean> BEAN_ATTR =
                AttributeKey.valueOf(MyAttributeKeys.class, "BEAN_ATTR");
        ...
    }

Then, you can access them via ``RequestContext.attr(AttributeKey)``:

.. code-block:: java

    // Setting
    ctx.setAttr(INT_ATTR, 42);
    MyBean myBean = new MyBean();
    ctx.setAttr(BEAN_ATTR, new MyBean());

    // Getting
    Integer i = ctx.attr(INT_ATTR); // i == 42
    MyBean bean = ctx.attr(BEAN_ATTR); // bean == myBean

You can also iterate over all the attributes in a context using ``RequestContext.attrs()``:

.. code-block:: java

    ctx.attrs().forEachRemaining(e -> {
        System.err.println(e.getKey() + ": " + e.getValue());
    });
