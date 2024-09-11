# Part 2

```C#

namespace MVC.Controllers;

public class User {
    public int Id { get; set; }
    public string FirstName { get; set;}
    public string LastName { get; set;}
    public int Age {get; set;}
}

[ApiController]
[Route("user/[controller]")
public class UserController : Controller
{
    private static List<User> users = new List<User>();
    
    [HttpGet]
    [Route("GetUsers")
    public IActionResult GetUsers()
    {
        return View(users);
    }
    
    [HttpPost]
    [Route("AddOrUpdateUser")]
    public ActionResult<User> AddOrUpdateUser([FromBody] User user)
    {
        if (user == null) return BadRequest("User data is null");
        
        //  first search for the user
        var foundUser = users.Find(usr => user.Id == user.Id);
        
        // if the user is not found add it
        if (foundUser != null)
        {
            foundUser.FirstName = user.FirstName;
            foundUser.LastName = user.LastName;
            foundUser.Age = user.Age;
            
            return Ok("User updated")
        } else {
            // update the found user
            users.Add(user);
            
            // return the update user object
            return CreatedAtAction(nameof(GetUser), new { id = user.Id ), user);
        }
    }
}
```
