# Coding Guidelines
See more about PHP/Laravel coding guidelines on wiki: https://github.com/mayanksdudakiya/coding-guidelines/wiki

#### 1) Avoid hard coded table name in `assertDatabaseHas` test method

**Before:**

`$this->assertDatabaseHas('users', $newlyAddedUser);`

**After:**

`$this->assertDatabaseHas(User::class, $newlyAddedUser);`


#### 2) Never use hardcoded string for comparision or anywhere else instead use Constant, Enum, Language file

**Before:**

```
if ($status === 'active') {
    // executre true
}
```

**After:**

```
enum Status: String
{
    case ACTIVE = 'active';
    case INACTIVE = 'inactive';
}


if ($status === Status::ACTIVE->value) {
    // executre true
}
```
 

#### 3) Select only required columns in Eloquent queries, this will help you to improve query performance

**Before:**

```
public function index(): View
{
  $users = User::all();
  return view('users', compact($users));
}
```

**After:**

```
public function index(): View
{
  $users = User::get(['id', 'name', 'email', 'phone']);
  return view('users', compact($users));
}
```

#### 4) Pass only required parameter into functions, components, it will help you to improve performance

In below example, we are passing `User` collection while we only need `user_id`. So just pass only what we need, this will improve dependencies as well as performance.

**Reference:** https://codecourse.com/watch/livewire-performance?part=passing-basic-prop-data-instead-of-huge-objects

**Function example**

**Before:**

```
public function getProductsByUserId(User $user): Collection
{
  return Product::where('user_id', $user->id)->get();
}
```

**After:**

```
public function getProductsByUserId(int $userId): Collection
{
  return Product::where('user_id', $user->id)->get();
}
```

**Component example**

Let's say, we have `employees` database table with `id`, `name`, `description`, `designation`, `phone`, `address` fields. So in the before example we are passing whole employee records but we only require `ID` & `Name` to print.

This will take nearly **_650ms_**.

**Before:**
 
```
@foreach ($employees as $employee)
    <livewire:employee-list-item :employee="$employee">
@endforeach
```

But after changing above to below, now it will take nearly **_325ms_**, nearly half request time. So you can imagine, passing unnecessary data in components can cause huge performance issue.

**After:**

```
@foreach ($employees as $employee)
    <livewire:employee-list-item :employee-id="$employee->id" :employee-name="$employee->name">
@endforeach
```
