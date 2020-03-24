{code0}Send{/code0} Approximation

Some {code0}async fn{/code0} state machines are safe to be sent across threads, while
others are not. Whether or not an {code1}async fn{/code1} {code2}Future{/code2} is {code3}Send{/code3} is determined
by whether a non-{code4}Send{/code4} type is held across an {code5}.await{/code5} point. The compiler
does its best to approximate when values may be held across an {code6}.await{/code6}
point, but this analysis is too conservative in a number of places today.

For example, consider a simple non-{code0}Send{/code0} type, perhaps a type

which contains an {code1}Rc{/code1}:

Variables of type {code0}NotSend{/code0} can briefly appear as temporaries in {code1}async fn{/code1}s
even when the resulting {code2}Future{/code2} type returned by the {code3}async fn{/code3} must be {code4}Send{/code4}: