#### The 11 Truths of Debugging 

1. Intuition and hunches are great—you just have to test them out. When a hunch and a fact collide, the fact wins. That's life in the city.
2. Don’t look for complex explanations. Even the simplest omission or typo can lead to very weird behavior.  Everyone is capable producing extremely simple and obvious errors from time to time. Look at code critically—don’t just sweep your eye over that series of simple statements assuming that they are too simple to be wrong.
3. The clue to what is wrong in your code is in the values of your variables and the flow of control. Try to see what the facts are pointing to. The computer is not trying to mislead you. Work from the facts.
4. Be systematic and persistent. Don’t panic. The bug is not moving around in your code, trying to trick or evade you. It is just sitting in one place, doing the wrong thing in the same way every time. 
5. If you code was working a minute ago, but now it doesn’t—what was the last thing you changed? This incredibly reliable rule of thumb is the reason your section leader told you to test your code as you go rather than all at once.
6. Do not change your code haphazardly trying to track down a bug. This is sort of like a scientist who changes more than one variable in an experiment at a time. It makes the observed behavior much more difficult to interpret, and you tend to introduce new bugs.
7. If you find some wrong code that does not seem to be related to the bug you were tracking, fix the wrong code anyway. Many times the wrong code was related to or obscured the bug in a way you had not imagined.
8. You should be able to explain in Sherlock Holmes style the series of facts, tests, and deductions that led you to find a bug. Alternately, if you have a bug but can’t pinpoint it, then you should be able to give an argument to a critical third party detailing why each one of your functions cannot contain the bug. One of these arguments will contain a flaw since one of your functions does in fact contain a bug. Trying to construct the arguments may help you to see the flaw.
9. Be critical of your beliefs about your code. It’s almost impossible to see a bug in a function when your instinct is that the function is innocent. Only when the facts have proven without question that the function is not the source of the problem should you assume it to be correct.
10. Although you need to be systematic, there is still an enormous amount of room for beliefs, hunches, guesses, etc. Use your intuition about where the bug probably is to direct the order that you check things in your systematic search. Check the functions you suspect the most first. Good instincts will come with experience.
11. Debugging depends on an objective and reasoned approach. It depends on overall perspective and understanding of the workings of your code. Debugging code is more mentally demanding than writing code. The longer you try to track down a bug without success, the less perspective you tend to have. Realize when you have lost the perspective on your code to debug. Take a break. Get some sleep. You cannot debug when you are not seeing things clearly. Many times a programmer can spend hours late at night hunting for a bug only to finally give up at 4:00A.M. The next day, they find the bug in 10 minutes.
 What allowed them to find the bug the next day so quickly? Maybe they just needed some sleep and time for perspective. Or maybe their subconscious figured it out while they were asleep. In any case, the “go do something else for a while, come back, and find the bug immediately” scenario happens too often to be an accident.