# Wetube Reloaded

## Router design

### Global router

/ -> Home
/join -> Join
/login -> Login
/search -> Search

### User router

/users/:id -> See User
/users/logout -> Log out
/users/edit -> Edit MY Profile
/users/delete -> Delete MY Profile

### Video router

/videos/:id -> See Video (/videos/watch)
/videos/:id/edit -> Edit Video
/videos/:id/delete -> Delete Video
/videos/upload -> Upload Video

/videos/comments ->Comment on a Video
/videos/comments/delete -> Delete a comment of a Video
