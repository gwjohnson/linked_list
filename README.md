#ifndef LAST_F_LIST
#define LAST_F_LIST
typedef unsigned int uint;

#include <cassert>
#include <sstream>

using namespace std;

/**
 * a simplified generic singly linked list class to illustrate C++ concepts
 * @author Jon Beck
 * @author Garland Johnson
 * @version 15 February 2017
 */
template< typename Object >
class List
{
private:
    /**
     * A class to store data and provide a link to the next node in the list
     */
    class Node
    {
public:
        /**
         * The constructor
         * @param data the data to be stored in this node
         */
        explicit Node( const Object& data )
            :data{ data }, next{ nullptr } {}

        Object data;
        Node* next;
    };

public:
    /**
     * The constructor for an empty list
     */
    List( )
        :size{ 0 }, first{ nullptr }, last{ nullptr } {}

    /**
     * the copy constructor
     */
    List( const List& rhs )
        :size{ 0 }, first{ nullptr }, last{ nullptr }
    {
        count = 0;
        // Node* list_iterator = rhs.first;

/** for loop grabs data from original list and uses a new function, push_back
 * to populate the new linked list with the original's data members.
 */
        for( auto list_iterator = rhs.first; list_iterator != nullptr;
             list_iterator = list_iterator->next )
        {
            count++;
            push_back( list_iterator->data );
            count++;
        }
    }

    /*swap written to allow a list to be copied into itself.(...and then
     * commented out, code left in to potentially reuse)
     *
     *
     *
     * void swap( List& rhs )
     * {
     *  auto t_size = size;
     *  auto t_first = first;
     *  auto t_last = last;
     *
     *  size = rhs.size;
     *  first = rhs.first;
     *  last = rhs.last;
     *
     *  rhs.size = t_size;
     *  rhs.first = t_first;
     *  rhs.last = t_last;
     * }
     *
     * operator= uses List constructor to avoid uneccessary code and written
     * swap
     * function. to copy one list into another.
     * --code left in to potentially reuse, after reading the prompt again,
     * definately the wrong way to complete assignment--
     *
     * List& operator=( const List& rhs )
     * {
     *  List new_list( rhs );
     *  swap( new_list );
     *
     *  return *this;
     * }
     */

    // operator= method safe to copy a list to itself.
    List& operator=( const List& rhs )
    {
        /*check to see if the first node of rhs = to first node of list, if it
         * is then a copy of itself is being made and rhs is returned.
         * if not set up in this manner gives "control reaches end of non-void
         * function"
         */
        count = 0;
        if( rhs.first != first )
        {
            while( !is_empty( ) )
            {
                pop_front( );
                count++;
            }
            for( auto list_iterator = rhs.first; list_iterator != nullptr;
                 list_iterator = list_iterator->next )
            {
                count++;
                push_back( list_iterator->data );
            }
        }

        /* if not a copy of itself, iterate through list to remove nodes and
         * push nodes from list onto back of new list. Code modified from
         * original list constructor and used in operator=
         */
        return *this;
    }

    /**
     * The destructor that gets rid of everything that's in the list and
     * resets it to empty. If the list is already empty, do nothing.
     *
     * Next time: use a for loop, as we know how big the list is
     *
     */
    ~List( )
    {
        if( size != 0 )
        {
            Node* current = first;
            Node* temp;

            while( current != nullptr )
            {
                temp = current;
                current = current->next;
                delete temp;
            }
        }
    }

    /**
     * Put a new element onto the beginning of the list
     * @param item the data the new element will contain
     */
    void push_front( const Object& item )
    {
        count = 0;
        /** Function first checks the first node to see if the list is empty
         * if the list is empty it sets the first and last node pointers to the
         * created Node.
         */

        if( first == nullptr )
        {
            last = first = new Node { item };
        }
        /** If list is not empty create new node and insert before first node
         * pointing to the old "first Node" (now second node).
         */
        else
        {
            count++;
            Node* new_node = new Node { item };
            new_node->next = first;

            first = new_node;
        }
        size++;
        count++;
    }

    /**
     * Put a new element onto the end of the list
     * @param item the data the new element will contain
     */
    void push_back( const Object& item )
    {
        /** Function first checks the first node to see if the list is empty
         * if the list is empty it sets the first and last node pointers to the
         * created Node.
         */
        count = 0;
        if( first == nullptr )
        {
            last = first = new Node { item };
        }
        /** If list is not empty create new node and insert before first node
         * pointing to the old "first Node" (now second node).
         */
        else
        {
            Node* new_node = new Node { item };
            last->next = new_node;
            last = new_node;
        }
        size++;
        count++;
    }

    /**
     * Remove the element that's at the front of the list. Causes an
     * assertion error if the list is empty.
     */
    void pop_front( )
    {
        assert( !is_empty( ) );
        Node* temp = first;

        if( first == last )
        {
            first = last = nullptr;
        }
        else
        {
            first = first->next;
        }
        delete temp;
        size--;
    }

    /**
     * Accessor to return the data of the element at the front of the list.
     * Causes an assertion error if the list is empty.
     * @return the data in the front element
     */
    const Object & front( ) const
    {
        assert( !is_empty( ) );

        return first->data;
    }

    /**
     * Accessor to return the data of the element at the tail of the list.
     * Causes an assertion error if the list is empty.
     * @return the data in the last element
     */
    const Object & tail( ) const
    {
        assert( !is_empty( ) );

        return last->data;
    }

    /**
     * Accessor to determine whether the list is empty
     * @return a boolean corresponding to the emptiness of the list
     */
    bool is_empty( ) const
    {
        return size == 0;
    }

    /**
     * Generate a string representation of the list
     * Requires operator<< to be defined for the list's object type
     * @return string representation of the list
     */
    string to_string( ) const
    {
        if( size == 0 )
        {
            return "";
        }
        stringstream buffer;
        for( auto current = first; current != nullptr; current = current->next )
        {
            buffer << current->data << ' ';
        }

        return buffer.str( );
    }

    uint get_count( )
    {
        return count;
    }

private:
    uint size;
    Node* first;
    Node* last;
    uint count;
};

#endif
