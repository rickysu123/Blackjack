# detailed implementation of blackjack, follows classic casino rules
# dealer hits on "soft 17"
# dealer stands on 17 or more
# dealer's first card is down

# player goes first (or player1 if multiple plyers)

# to do
# include a hint probability function
# include bets
# insurance (when dealer's face up is Ace)
# split
# double down
# multiple players

import random

# OO of a player
# has a couple of helpful quick methods
class player():
    def __init__(self, firstCard, secondCard, person): # person is dealer or player
        self.first = firstCard
        self.second = secondCard
        self.total = self.initialTotal()
        self.person = person
        self.cards = [getCardName(firstCard), getCardName(secondCard)]

    def initialTotal(self): # calculates total, because face card values are 10
        first = self.first
        second = self.second
        if first > 10:
                first = 10
        if second > 10:
                second = 10
        return first + second

    def getFirst(self):
        return self.first

    def getSecond(self):
        return self.second

    def getPerson(self):
        return self.person

    def getTotal(self):
        return self.total

    def getCards(self):
        return self.cards

    def printCards(self): # prints every card the player has
        print("Your cards are: ", end = "")
        allCards = []
        for card in self.cards:
            allCards.append(card)
        print(", ".join(allCards))
        

    def add(self, card):
        self.cards.append(getCardName(card))
        self.total += getRealValue(card)

    def hasBlackjack(self):
        # returns true if player gets 21, counts Ace as 11 to check
        if self.first == 1:
            if getRealValue(self.second) + 11 == 21:
                return True
        if self.second == 1:
            if getRealValue(self.first) + 11 == 21:
                return True
        return False

    def hasAce(self):
        # checks if player has an Ace
        if 1 in self.getCards():
            return True
        return False

    def checkBust(self):
        # if Ace is counted as 1 and player still busts, then if Ace is counted as 11, player will bust
        if self.total > 21:
            return True
        return False
    

##########################
# IMPORTANT FUNCTIONS #

def getRealValue(card): # changes a face card to value 10 if necessary
    if card > 10:
        return 10
    return card


def getCardName(card): # for specifying what card it is (special cases)
        dic = {1:"Ace", 11:"Jack", 12:"Queen", 13:"King"}
        if card > 10:
            return dic[card]
        elif card == 1:
            return dic[card]
        return str(card)

def getRandomCard(cards): # get a random card and return new deck
    index = random.randrange(len(cards)) # get random card
    card = cards[index]
    del cards[index] # remove card from list of all cards
    return card, cards

# IMPORTANT FUNCTIONS #
##########################

# FUNCTIONS FOR USER AND DEALER TURNS #
def userTurn(user, cards):
    while True:
        decision = input("Would you like to hit or stand? (\'h\' or \'s\'): ")
        if decision is 'h': # hit
            card, cards = getRandomCard(cards)
            print("You drew a %s." % getCardName(card))
            user.add(card)
            user.printCards()
            if user.checkBust():
                return 0, cards # return total of 0 for busted
        else: # stand
            if user.hasAce():
                decision = input("Would you like to count an Ace as 1 or 11? (\'1\' or \'11\'): ")
                if decision == "11":
                    user.total += 10
                if user.checkBust():
                    return 0, cards # return total of 0 for busted
            break
    return user.total, cards


def checkSoft17(dealer):
    # has only one Ace and totals 17 
    # will hit
    if 1 in dealer.getCards() and dealer.total == 17:
        return True
    return False


def soft17(dealer, cards):
    print("enter")
    # handle soft 17 situation
    while dealer.getTotal() < 17:
        card, cards = getRandomCard(cards) # get random card
        dealer.add(getRealValue(card))
        print("Dealer hit. Dealer drew a %s." % getCardName(card))
        if dealer.getTotal() + 10 <= 21:
            # if the first card drawn causes total to be 21 or less, dealer stands
            return dealer.getTotal() + 10, cards
        if dealer.checkBust(): # check if dealer busts
            return 0, cards

# FUNCTIONS FOR USER AND DEALER TURNS #
##########################

#def play(cards, betting, money):
def play(cards):
    ##########################
    # INITIAL DEAL #
    initialCards = []
    for i in range(4):
        index = random.randrange(len(cards)) # get random card
        initialCards.append(cards[index])
        del cards[index] # remove card from list of all cards
    # give cards to dealer and player
    dealer = player(initialCards[0], initialCards[2], "dealer")
    user = player(initialCards[1],initialCards[3], "player")
    ##########################
    # TELL USER THE CARDS DRAWN #
    print("Dealer has a face up %s." % (getCardName(dealer.getSecond()))) # only dealer's second card is face up
    user.printCards()
    ##########################
    # INITIAL CHECK FOR BLACKJACKS #
    if dealer.hasBlackjack() and not user.hasBlackjack(): # dealer has Blackjack (has 21)
        print("Dealer's face down card is a %s." % getCardName(dealer.first))
        print("Dealer drew 21! You lose!")
        return # exit function
    elif user.hasBlackjack() and not dealer.hasBlackjack(): # player has Blackjack (has 21)
        print("You drew 21! You win!")
        return # exit function
    elif user.hasBlackjack() and dealer.hasBlackjack(): # push, both Blackjacks
        print("You and the dealer drew 21! Push! No one wins.")
        return # exit function
    ##########################
    # USER'S TURN FIRST #
    user.total, cards = userTurn(user, cards)
    if user.total == 0:
        # user busted
        print("You busted! You lose.")
        return
    # END USER'S TURN #
    ##########################
    # DEALER'S TURN #
    # soft17 check first
    if checkSoft17(dealer):# dealer has a "soft 17" (dealer has 17 with an Ace), will hit
        # this function will finish the turn
        dealer.total, cards = soft17(dealer, cards)
        if dealer.total == 0:
            # dealer busted
            print("Dealer busts with a total of %s. You win!" % str(dealer.getTotal()))
            return
    else:
        # usual algorithm
        while dealer.total < 17:
            card, cards = getRandomCard(cards)
            print("Dealer hit. Dealer drew a %s." % getCardName(card))
            dealer.add(getRealValue(card))
            if dealer.checkBust():
                print("Dealer busts with a total of %s. You win!" % str(dealer.getTotal()))
                break
            # if dealer gets a soft17
            if checkSoft17(dealer):# dealer has a "soft 17" (dealer has 17 with an Ace), will hit
                # this function will finish the turn
                dealer.total, cards = soft17(dealer, cards)
                if dealer.total == 0:
                    # dealer busted
                    print("Dealer busts with a total of %s. You win!" % str(dealer.getTotal()))
                    return
                break
            # if dealer gets a soft17
    # END DEALER'S TURN #
    ##########################
    # CHECK WINNER #
    print("Your total: %d \n Dealer total: %d" % (user.getTotal(), dealer.getTotal()))
    if user.getTotal() > dealer.getTotal():
        print("You win!")
    elif dealer.getTotal() > user.getTotal():
        print("Dealer wins!")
    else:
        print("Push! No one wins!")


def main():
    cards = []
    # Aces are listed as 1, but can count as 11
    ##########################
    # ADDITIONAL OPTIONS #
    # MULTIPLE DECKS
    numberOfDecks = int(input("How many decks would you like to play with? "))
    '''# BETTING
    betting = input("Would you like to bet money? (\'y\' or \'n\'): ")
    money = 0
    if betting is 'y':
        money = float(input("How much money would you like to start with? "))'''
    # ADDITIONAL OPTIONS #
    ##########################
    for i in range(1,14):
        cards.extend([i] * (4*numberOfDecks))
    while True:
        money = play(cards, betting, money)
        again = input("\nWould you like to play again? (\'y\' or \'n\'): ")
        if again is not 'y':
            print("Thanks for playing!")
            break;


main()
