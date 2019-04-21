# David - April 21, 2019
## Installation
1. Clone the repository. The one repository contains both the API server and the UI code.
2. Start the API server. From the repository root: `cd apiserver && yarn start`. This will start nodemon and serve the basic API from port 8000.
3. Start the UI development server. From the repository root: `cd ui && yarn start`. This will start the UI server on port 3000.

## Security
*Concerns I've addressed:*
- Uploaded files are automatically renamed. The original file name is stored in metadata.
- The path a file is stored on disk is never exposed to the end user.
- User input is sanitized. File names, search parameters, etc. are treated as untrusted.
- User input is never used for display purposes. Error messages and other feedback use standard messages.
- I've checked included dependencies for security problems.

*Concerns that need to be addressed:*
- The API currently doesn't verify that a file is actually a PNG or JPG. You could upload an executable file.
- The API doesn't remove exif data from JPG images.
- Files uploaded via the API are stored directly on disk. A copy should be made and then that version is what is actually stored.
- Uploaded files should be virus scanned.
- The entire API and  should be scan
- Both projects should be containerized and run by a user account with minimal privileges. Docker runs everything as root, for example.

## Improvements
- A real database. To save time I used the filesystem to store metadata about images. This should have been stored in a relational database or a document store. This is my first Express based app and I didn't want to incur the overhead in time of learning how to add some sort of ORM and database support.
- Tighter security. Files need to be scanned and verified to be harmless. My security concerns are noted above.
- Use third party hosting for uploaded files like S3.
- API unit tests
- UI functional tests
- More UI unit tests
- The API could allow users to include metadata with a file such as a title, description, comments, etc.
- An admin facility for working with uploads.
- The repository for both projects should be separated into two repositories.

## Libraries
**API project dependencies:**

`body-parser` this is a utility that allows express to get request parameters easily\
`cors` cross origin request middleware for express\
`express` server\
`multer` a tool for handling file uploads in express\
`nodemon` a tool for watching a running node process and restarting when code changes

**UI project dependencies**

`prop-types` react component proptypes\
`react` ui library\
`react-dom` react dependency\
`react-scripts` react dependency\
`enzyme` a react testing library\
`enzyme-adapter-react-16` enzyme dependency. adapts enzyme to a specific react version.

## API
*Any general observation about the API?*

It's a real challenge to secure this API. I've tried to note everything I wanted to do but didn't have time for.
### GET /listuploads
```
List all uploads.
returns: an array of objects matching the shape:
{
  id: String,
  size: Number,
  name: String
}
```
### GET /listuploads/:name
```
List all uploads filtered by the original filename.
params:
- name: A string value to search original filenames with. Case sensitive.
returns:
- an array of objects matching the shape:
{
  id: String,
  size: Number,
  name: String
}
```
### POST /upload
```
Upload a file of type png or jpg.
params: data
returns: an object matching the shape:
{
  id: String,
  size: Number,
  name: String
}
```
### DELETE /removeupload/:fileId
```
Remove an uploaded file.
params:
- fileId: an exact file ID 
returns:
- an object matching the shape:
{
  id: String,
}
```
---
## Other notes
I could have spent days and days working on this. The API layer is a fun challenge on its own.
