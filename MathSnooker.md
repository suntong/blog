# Testing your math and programming skills

[category Tech][tags programming,go]

Math problems can easily be very hard. Finding some math programming tasks that are simple enough to be finished in a short time, yet interesting enough in the meantime is not that easy. Today I've found another one. Here it is:

<!--more-->

Given two random balls on a snooker table, write an algorithm that will make one ball hit the other one **after** bouncing off **twice** from the sides of the snooker table. Mark down the trace that it should travel to make the hit.

There should be a bit of physics knowledge there (the rule in reflection), but to truly solve the problem beautifully, the math skill is a must.

To make it easy, the result is shown in text based. Suppose the snooker table is 80 by 60, so the snooker table, the balls and the trace should be drawn within a 80 by 60 characters space. Here are two examples:

```
##################################O#############################################
#                                O O                                           #
#                               O   O                                          #
#                               O   O                                          #
#                              O     O                                         #
#                             O       O                                        #
#                            O         O                                       #
#                           O           O                                      #
#                           O           O                                      #
#                          O             O                                     #
#                         O               O                                    #
#                        O                 O                                   #
#                       O                   O                                  #
#                       O                   O                                  #
#                      O                     O                                 #
#                     O                       O                                #
#                    O                         O                               #
#                   O                           O                              #
#                   O                           O                              #
#                  O                             O                             #
#                 O                               O                            #
#                O                                 O                           #
#               O                                   O                          #
#               O                                   O                          #
#              O                                     O                         #
#             O                                       O                        #
#            O                                         O                       #
#           O                                           O                      #
#           O                                           O                      #
#          O                                             O                     #
#         O                                               O                    #
#        O                                                 O                   #
#       O                                                   O                  #
#       O                                                   O                  #
#      O                                                     O                 #
#     O                                                       O                #
#    O                                                         O               #
#   O                                                           O              #
#   O                                                           O              #
#  O                                                             O             #
# O                                                               O            #
#O                                                                 O           #
O                                                                   O          #
O                                                                   O          #
#O                                                                   O         #
# O                                                                   O        #
#  O                                                                   O       #
#   O                                                                   O      #
#   O                                                                   X      #
#    O                                                                         #
#     O                                                                        #
#      O                                                                       #
#       O                                                                      #
#       O                                                                      #
#        O                                                                     #
#         O                                                                    #
#          O                                                                   #
#           X                                                                  #
#                                                                              #
################################################################################
```

and a partial one:

```
#############################O##################################################
#                         O    O                                               #
#                       O         O                                            #
#                    O              O                                          #
#                  O                   O                                       #
#               O                        O                                     #
#             O                             O                                  #
#          O                                  O                                #
#        O                                       O                             #
#     O                                            O                           #
#   O                                                 O                        #
#O                                                      O                      #
#O                                                         O                   #
#   O                                                        O                 #
#     O                                                         X              #
#        O                                                                     #
#          O                                                                   #
#             O                                                                #
#               O                                                              #
#                 X                                                            #
#                                                                              #
#                                                                              #
#                                                                              #
#                                                                              #
#                                                                              #
...

```
