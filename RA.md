//1 Find all pizzas eaten by at least one female over the age of 20. 
\project_{pizza} (
    (\select_{age > '20' and gender='female'} Person)
    \join Eats
);

//2 Find the names of all females who eat at least one pizza served by Straw Hat. (Note: The pizza need not be eaten at Straw Hat.) 
\project_{name} (
    \project_{pizza}
        (\select_{pizzeria = 'Straw Hat'} Serves)
    \join Eats // ate pizza
    \join (\select_{gender = 'female'} Person)
);

//3 Find all pizzerias that serve at least one pizza for less than $10 that either Amy or Fay (or both) eat.
\project_{pizzeria} (
    \project_{pizza}
        (\select_{name = 'Amy' or name = 'Fay'} Eats)
    \join (\select_{price < '10'} Serves)
);

//4 Find all pizzerias that serve at least one pizza for less than $10 that both Amy and Fay eat. 
\project_{pizzeria} (
    \project_{pizza}
        (\select_{name = 'Amy'} Eats)
    \intersect
    \project_{pizza}
        (\select_{name = 'Fay'} Eats)
   
    \join (\select_{price < '10'} Serves)
);

//5 Find the names of all people who eat at least one pizza served by Dominos but who do not frequent Dominos. 
\project_{name} (
    (\select_{pizzeria = 'Dominos'} Serves) 
    \join Eats
)
\diff
\project_{name} (
    \select_{pizzeria = 'Dominos'} Frequents
)

//6 Find all pizzas that are eaten only by people younger than 24, or that cost less than $10 everywhere they're served. 
(\project_{pizza} Serves
\diff
\project_{pizza}
    (\select_{price >= 10} Serves))
    
\union

(\project_{pizza} Eats
\diff
\project_{pizza}
    (\select_{age >= 24} Person 
     \join Eats)
)