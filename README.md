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
public function index() {
  $users = User::all();
  return view('users', compact($users));
}
```

**After:**

```
public function index() {
  $users = User::get(['id', 'name', 'email', 'phone']);
  return view('users', compact($users));
}
```
