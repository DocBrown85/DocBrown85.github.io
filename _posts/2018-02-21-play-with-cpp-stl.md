---
layout: post
title: Playing with C++ STL
---

~~~
#include <algorithm>    // for std::find
#include <iostream>     
#include <deque>        // for std::deque
#include <list>         // for std::list
#include <map>          // for std::map
#include <memory>       // for smart pointers
#include <string>       // for std::string
#include <utility>      // for std::pair
#include <vector>       // for std::vector

/*
 * In order for a class to be stored in an STL container, it must have:
 *
 * - a default constructor
 * - must be assignable
 * - less than comparable
 * - equality comparable
 *
 */
class aType {
    
    // all public here
    public:
    
    // a member
    std::string aString;
    
    // default constructor
    aType() {}
    
    // another constructor
    aType(std::string str) : aString(str) {}
    
    // copy constructor
    aType(const aType& t) {
        aString = t.aString;
    }
    
    // move constructor (takes an rvalue reference:
    // an rvalue is an unnamed value that exists
    // only during the evaluation of an expression)
    aType(aType&& t) {
        aString = t.aString;
    }
    
    // move operator (takes an rvalue reference:
    // an rvalue is an unnamed value that exists
    // only during the evaluation of an expression)
    aType& operator=(aType&& t) {
        aString = t.aString;
    }
    
    // assigment operator
    aType& operator=(const aType& t) {
        aString = t.aString;
    }
    
    // destructor
    virtual ~aType() {}
    
    // == operator
    int operator==(const aType& t) {
        return aString == t.aString;
    }
    
    // < operator
    int operator<(const aType& t) {
        return aString < t.aString;
    }
    
};

void playWithPair() {
    
    std::pair<std::string, int> aPair = make_pair(std::string("akiri"), 350);
    
    std::cout << "first is: " << aPair.first << std::endl;
    std::cout << "second is: " << aPair.second << std::endl;
    
}

void playWithVector() {
    
    /*
     * offers efficient insertion and removal at the end of the container,
     * also offers random access to its elements
     */
    
    // declare it
    std::vector<int> aVector;
    
    // fill some values
    for (int i=0; i<10; ++i) {
        aVector.push_back(i);
    }
    
    // iterate (forward)
    std::cout << "iterating forward" << std::endl;
    std::vector<int>::iterator it = aVector.begin();
    while(it != aVector.end()) {
        std::cout << *it << std::endl;
        ++it;
    }
    
    // iterate (backward)
    std::cout << "iterating backward" << std::endl;
    std::vector<int>::reverse_iterator rit = aVector.rbegin();
    while(rit != aVector.rend()) {
        std::cout << *rit << std::endl;
        ++rit;
    }
    
    // get size
    size_t size = aVector.size();
    std::cout << "size is: " << size << std::endl;
    
    // get capacity
    size_t capacity = aVector.capacity();
    std::cout << "capacity is: " << capacity << std::endl;
    
    // remove from back
    aVector.pop_back();
    
    // insert on top
    std::vector<int>::iterator start = aVector.begin();
    aVector.insert(start, 100);
    
    // insert somewhere
    std::vector<int>::iterator insertPosition = aVector.begin();
    insertPosition += 4;
    aVector.insert(insertPosition, 1234);
    
    // delete somewhere
    std::vector<int>::iterator erasePosition = aVector.begin();
    erasePosition += 3;
    aVector.erase(erasePosition);
    
    std::cout << "modified vector" << std::endl;
    std::vector<int>::iterator fit = aVector.begin();
    while(fit != aVector.end()) {
        std::cout << *fit << std::endl;
        ++fit;
    }
    
}

void playWithDeque() {
    
    /*
     * offers the same API as vector, but also
     * implements efficient insertion and removal
     * at the beginning of the sequence with:
     *
     * push_front()
     *
     * pop_front()
     *
     */
    
}

void playWithList() {
    
    /*
     * offers efficient inertion and removal anywhere, but does not
     * allow random access.
     *
     * std::list<int>::iterator it = std::find(aList.begin(), aList.end(), 1);
     */
     
    // declare it
    std::list<aType> aList;
    
    for (int i=0; i<10; ++i) {
        aList.push_front(
            aType(std::string("my value is: " + std::to_string(i)))
        );
    }
    
    std::list<aType>::iterator it = aList.begin();
    while(it != aList.end()) {
        std::cout << it->aString << std::endl;
        ++it;
    }
    
    // search an item (will find it)
    aType itemToFind(std::string("my value is: " + std::to_string(4)));
    std::list<aType>::iterator position = std::find(
        aList.begin(),
        aList.end(),
        itemToFind
    );
    
    if (position != aList.end()) {
        std::cout << "item found:" << std::endl;
        std::cout << position->aString << std::endl;
    }
    else {
        std::cout << "item not found" << std::endl;
    }
    
    // search an item (will not find it)
    aType iWontBeFound("I wont be found");
    std::list<aType>::iterator res = std::find(
        aList.begin(),
        aList.end(),
        iWontBeFound
    );
    
    if (res != aList.end()) {
        std::cout << "item found" << std::endl;
    }
    else {
        std::cout << "item not found" << std::endl;
    }
    
}

void playWithMap() {
    
    // declare it
    std::map<std::string, int> aMap;
    
    // fill some values
    std::pair<std::map<std::string, int>::iterator, bool> result =
    aMap.insert(make_pair(std::string("akiri"), 100));
    if (!result.second) {
        std::cout << "the value is already in the map!" << std::endl;
    }
    else {
        std::cout << "the value has been added to the map!" << std::endl;
    }
    std::cout << "the stored key is: " << result.first->first << std::endl;
    std::cout << "the stored value is: " << result.first->second << std::endl;
    
    aMap.insert(make_pair(std::string("teneb"), 150));
    aMap.insert(make_pair(std::string("erebos"), 200));
    aMap.insert(make_pair(std::string("narset"), 0));
    aMap["mayael"] = 250;
    aMap["ur-dragon"] = 300;
    aMap["brago"] = 0;
    
    // check insertion
    std::pair<std::map<std::string, int>::iterator, bool> checkInsert =
    aMap.insert(make_pair(std::string("akiri"), 100));
    if (!checkInsert.second) {
        std::cout << "the value is already in the map!" << std::endl;
    }
    else {
        std::cout << "the value has been added to the map!" << std::endl;
    }
    
    // iterate (forward)
    std::cout << "iterating forward" << std::endl;
    std::map<std::string, int>::iterator forwardIt = aMap.begin();
    while (forwardIt != aMap.end()) {
        std::cout << "aMap[" << forwardIt->first << "]=" << forwardIt->second << std::endl;
        ++forwardIt;
    }
    
    // iterate (backward)
    std::cout << "iterating backward" << std::endl;
    std::map<std::string, int>::reverse_iterator backwardIt = aMap.rbegin();
    while (backwardIt != aMap.rend()) {
        std::cout << "aMap[" << backwardIt->first << "]=" << backwardIt->second << std::endl;
        ++backwardIt;
    }
    
    // find a value
    std::map<std::string, int>::iterator res = aMap.find("teneb");
    if (res != aMap.end()) {
        std::cout << "found value:" << std::endl;
        std::cout << "aMap[" << res->first << "]=" << res->second << std::endl;
    }
    else {
        std::cout << "value not found:" << std::endl;
    }
    
    // erase a value by iterator
    std::cout << "erasing brago from the map" << std::endl;
    std::map<std::string, int>::iterator toErase = aMap.find("brago");
    aMap.erase(toErase);
    
    // erase a value by key
    std::cout << "erasing narset from the map" << std::endl;
    aMap.erase("narset");
    
    std::cout << "new map values" << std::endl;
    for (auto iter = aMap.begin(); iter != aMap.end(); ++iter) {
        std::cout << "aMap[" << iter->first << "]=" << iter->second << std::endl;
    }
    
}

void playWithSmartPointers() {
    
    // create a shared pointer
    std::shared_ptr<aType> aSharedPtr =
        std::make_shared<aType>("I am a shared object!");
        
    // this can be done
    std::shared_ptr<aType> anotherSharedPtr = aSharedPtr;
    
    // get the underlying raw pointer
    aType* rawPtr = aSharedPtr.get();
    
    // switch the pointed content with a new one
    // the previous object wont be deleted because anotherSharedPointer is
    // still holding it, but its reference count is decreased by 1
    aSharedPtr.reset(new aType("I am a new object"));
    
    // clear the content of anotherSharedPointer, now the old object is
    // deleted
    anotherSharedPtr.reset();
        
    // create a unique pointer
    std::unique_ptr<aType> aUniquePtr(new aType("I am a unique object!"));
    
    // this cannot be done: we get a compile error
    //std::unique_ptr<aType> anotherUniquePtr = aUniquePtr;
    
    // this can be done: transferring ownership
    std::unique_ptr<aType> yetAnotherUniquePtr(std::move(aUniquePtr));
    
}

int main() {
    
    std::cout << "Play with STL containers" << std::endl;
    
    playWithPair();
    
    playWithVector();
    
    playWithDeque();
    
    playWithList();
    
    playWithMap();
    
    playWithSmartPointers();

    return 0;
}
~~~
