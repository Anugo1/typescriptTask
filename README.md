# FilterPersons Function in TypeScript

## Overview
This project provides a `filterPersons` function that filters an array of users and admins based on given criteria. It ensures strict TypeScript type safety, making the function return either `User[]` or `Admin[]` based on the `personType` argument.

## Features
- **Type Safety**: Returns `User[]` when filtering users and `Admin[]` when filtering admins.
- **Flexible Filtering**: Allows partial filtering criteria.
- **Excludes `type` from Filtering**: Users cannot filter based on the `type` property.

## Data Structures
### Interfaces
```typescript
interface User {
    type: 'user';
    name: string;
    age: number;
    occupation: string;
}

interface Admin {
    type: 'admin';
    name: string;
    age: number;
    role: string;
}
```
These interfaces define two different types of persons:
- **User** has `name`, `age`, and `occupation`.
- **Admin** has `name`, `age`, and `role`.

### Union Type
```typescript
export type Person = User | Admin;
```
A `Person` can be either a `User` or an `Admin`.

### Sample Data
```typescript
export const persons: Person[] = [
    { type: 'user', name: 'Max Mustermann', age: 25, occupation: 'Chimney sweep' },
    { type: 'admin', name: 'Jane Doe', age: 32, role: 'Administrator' },
    { type: 'user', name: 'Kate Müller', age: 23, occupation: 'Astronaut' },
    { type: 'admin', name: 'Bruce Willis', age: 64, role: 'World saver' },
    { type: 'user', name: 'Wilson', age: 23, occupation: 'Ball' },
    { type: 'admin', name: 'Agent Smith', age: 23, role: 'Anti-virus engineer' }
];
```
This array stores different users and admins.

## Logging Function
```typescript
export function logPerson(person: Person) {
    console.log(
        ` - ${person.name}, ${person.age}, ${person.type === 'admin' ? person.role : person.occupation}`
    );
}
```
This function logs user details, displaying `occupation` for users and `role` for admins.

## `filterPersons` Function
```typescript
export function filterPersons<T extends 'user' | 'admin'>(
    persons: Person[],
    personType: T,
    criteria: Omit<Partial<T extends 'user' ? User : Admin>, 'type'>
): (T extends 'user' ? User[] : Admin[]) {
    return persons
        .filter((person): person is T extends 'user' ? User : Admin => person.type === personType)
        .filter((person) => {
            let criteriaKeys = Object.keys(criteria) as (keyof typeof criteria)[];
            return criteriaKeys.every((fieldName) => person[fieldName] === criteria[fieldName]);
        }) as any;
}
```
### Explanation:
1. **Generic Type (`T extends 'user' | 'admin'`)**:
   - This ensures that `personType` is either `'user'` or `'admin'`.
2. **Return Type**:
   - If `personType` is `'user'`, it returns `User[]`.
   - If `personType` is `'admin'`, it returns `Admin[]`.
3. **Filtering Logic**:
   - First, it filters `persons` based on `personType`.
   - Then, it filters using the `criteria` object.
   - It ensures `type` is excluded from filtering using `Omit<... , 'type'>`.

## Using `filterPersons`
```typescript
export const usersOfAge23 = filterPersons(persons, 'user', { age: 23 });
export const adminsOfAge23 = filterPersons(persons, 'admin', { age: 23 });
```
- `usersOfAge23` filters users who are `23` years old.
- `adminsOfAge23` filters admins who are `23` years old.

## Displaying Results
```typescript
console.log('Users of age 23:');
usersOfAge23.forEach(logPerson);

console.log();

console.log('Admins of age 23:');
adminsOfAge23.forEach(logPerson);
```
This prints the filtered results.

## Expected Output
```
Users of age 23:
 - Kate Müller, 23, Astronaut
 - Wilson, 23, Ball

Admins of age 23:
 - Agent Smith, 23, Anti-virus engineer
```

## Summary
✅ **Strict Type Safety** using generics.  
✅ **Prevents Filtering by `type`** for better data integrity.  
✅ **Dynamic Filtering** based on the provided criteria.  

This implementation ensures that the `filterPersons` function is robust and properly typed. 🚀

