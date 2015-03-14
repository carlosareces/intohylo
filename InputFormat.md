
```
% Input file in the format of hylolib 1.3

% first, define the signature of the language

signature
{

propositions { tall, strong, pretty, naive }
nominals     { alice, bob, jean, marie }
relations    { love,
               lovedBy : {inverseof love},
               know : {reflexive},
               touches : {symmetric},
               U : {universal},
               fatherOf,
               motherOf,
               olderThan : {transitive},
               parentOf : {equals {fatherOf,motherOf}, subsetof olderThan},
               childOf : {inverseof parentOf},
               youngerThan : {inverseof olderThan}
             }
}

% Then the theory.
% Just putting "true" accounts for an empty theory

theory {
 [U]((tall & strong) --> pretty);

 alice: ( strong  & !tall & !naive);
 bob: ( tall & !strong ) ;

 (alice:bob) v jean:<love>marie;

 bob:[lovedBy]naive;

 alice:<youngerThan>marie;
 marie:<youngerThan>bob;
 bob:<youngerThan>jean

 unknown:<parentOf>alice

}

% Then the reasoning tasks, done against the background theory.
% Here, "out1" and "out2" are the files where the (counter)-models will
% be written if they exist.

query (satisfiable? , "out1") {
 bob:<touches>jean;
 marie:<touches>jean;
 jean:[touches]naive

}

query (valid? , "out2") {
 alice:bob
}

query (retrieve , "retrieve1") {
 <youngerThan>jean
}

```