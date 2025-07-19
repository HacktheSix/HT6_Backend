supabase-py
Python client for Supabase

Documentation: supabase.com/docs
Usage:
GitHub OAuth in your Python Flask app
Python data loading with Supabase
Set up a Local Development Environment
Clone the Repository
git clone https://github.com/supabase/supabase-py.git
cd supabase-py
Create and Activate a Virtual Environment
We recommend activating your virtual environment. For example, we like poetry and conda! Click here for more about Python virtual environments and working with conda and poetry.

Using venv (Python 3 built-in):

python3 -m venv env
source env/bin/activate  # On Windows, use .\env\Scripts\activate
Using conda:

conda create --name supabase-py
conda activate supabase-py
PyPI installation
Install the package (for Python >= 3.9):

# with pip
pip install supabase

# with conda
conda install -c conda-forge supabase
Local installation
You can also install locally after cloning this repo. Install Development mode with pip install -e, which makes it editable, so when you edit the source code the changes will be reflected in your python module.

Usage
Set your Supabase environment variables in a dotenv file, or using the shell:

export SUPABASE_URL="my-url-to-my-awesome-supabase-instance"
export SUPABASE_KEY="my-supa-dupa-secret-supabase-api-key"
Init client:

import os
from supabase import create_client, Client

url: str = os.environ.get("SUPABASE_URL")
key: str = os.environ.get("SUPABASE_KEY")
supabase: Client = create_client(url, key)
Use the supabase client to interface with your database.

Sign-up
user = supabase.auth.sign_up({ "email": users_email, "password": users_password })
Sign-in
user = supabase.auth.sign_in_with_password({ "email": users_email, "password": users_password })
Insert Data
data = supabase.table("countries").insert({"name":"Germany"}).execute()

# Assert we pulled real data.
assert len(data.data) > 0
Select Data
data = supabase.table("countries").select("*").eq("country", "IL").execute()

# Assert we pulled real data.
assert len(data.data) > 0
Update Data
data = supabase.table("countries").update({"country": "Indonesia", "capital_city": "Jakarta"}).eq("id", 1).execute()
Update data with duplicate keys
country = {
  "country": "United Kingdom",
  "capital_city": "London" # This was missing when it was added
}

data = supabase.table("countries").upsert(country).execute()
assert len(data.data) > 0
Delete Data
data = supabase.table("countries").delete().eq("id", 1).execute()
Call Edge Functions
def test_func():
  try:
    resp = supabase.functions.invoke("hello-world", invoke_options={'body':{}})
    return resp
  except (FunctionsRelayError, FunctionsHttpError) as exception:
    err = exception.to_dict()
    print(err.get("message"))
Download a file from Storage
bucket_name: str = "photos"

data = supabase.storage.from_(bucket_name).download("photo1.png")
Upload a file
bucket_name: str = "photos"
new_file = getUserFile()

data = supabase.storage.from_(bucket_name).upload("/user1/profile.png", new_file)
Remove a file
bucket_name: str = "photos"

data = supabase.storage.from_(bucket_name).remove(["old_photo.png", "image5.jpg"])
List all files
bucket_name: str = "charts"

data = supabase.storage.from_(bucket_name).list()
Move and rename files
bucket_name: str = "charts"
old_file_path: str = "generic/graph1.png"
new_file_path: str = "important/revenue.png"

data = supabase.storage.from_(bucket_name).move(old_file_path, new_file_path)
Roadmap
 Wrap Postgrest-py

 Add remaining filters
 Add support for EXPLAIN
 Add proper error handling
 Use sanitize_param() to sanitize inputs.
 Fix client-side timeouts for long running queries.
 Enable HTTP2 by default.
 Enable follow redirects by default.
 Enable keep-alive by default.
 Enable running with unverified SSL via verify=False.
 Add Stalebot.
 Update CI (linters, etc).
 Check cyclomatic complexity and fix if needed (mccabe, prospector).
 Wrap Realtime-py

 Integrate with Supabase-py
 Support WALRUS
 Support broadcast (to check if already supported)
 Add close() method to close a socket.
 Add Stalebot.
 Update CI (linters, etc).
 Check cyclomatic complexity and fix if needed (mccabe, prospector).
 Wrap auth-py

 Remove references to GoTrue-js v1 and do a proper release
 Test and document common flows (e.g. sign in with OAuth, sign in with OTP)
 Add MFA methods
 Add SSO methods
 Add Proof Key for Code Exchange (PKCE) methods. Unlike the JS library, we do not currently plan to support Magic Link (PKCE). Please use the token hash in tandem with verifyOTP instead.
 Add is_anonymous boolean property.
 Add sign_in_with_id_token() method.
 Add sign_in_with_sso() method.
 Enable HTTP2 by default.
 Enable follow redirects by default.
 Enable keep-alive by default.
 Enable running with unverified SSL via verify=False.
 Add Stalebot.
 Update CI (linters, etc).
 Check cyclomatic complexity and fix if needed (mccabe, prospector).
 Wrap storage-py

 Support resumable uploads
 Setup testing environment
 Fix client-side timeouts for long running operations.
 Enable HTTP2 by default.
 Enable follow redirects by default.
 Enable keep-alive by default.
 Enable running with unverified SSL via verify=False.
 Add Stalebot.
 Update CI (linters, etc).
 Check cyclomatic complexity and fix if needed (mccabe, prospector).
 Document how to properly upload different file types (e.g. jpeg/png and download it)
 Wrap functions-py

 Fix client-side timeouts for long running functions.
 Enable HTTP2 by default.
 Enable follow redirects by default.
 Enable keep-alive by default.
 Enable running with unverified SSL via verify=False.
 Add Regions support.
 Add Stalebot.
 Update CI (linters, etc).
 Check cyclomatic complexity and fix if needed (mccabe, prospector).
Overall Tasks
 Add async support across the entire library
 Add FastAPI helper library (external to supabase-py)
 Add django-supabase-postgrest (external to supabase-py)
Contributing
Contributing to the Python libraries are a great way to get involved with the Supabase community. Reach out to us on Discord or on our Github Discussions page if you want to get involved.

Important: Proper Client Shutdown
To ensure the Supabase client terminates correctly and to prevent resource leaks, you must explicitly call:

client.auth.sign_out()
Running Tests
Currently, the test suites are in a state of flux. We are expanding our clients' tests to ensure things are working, and for now can connect to this test instance, which is populated with the following table:



The above test database is a blank supabase instance that has populated the countries table with the built-in countries script that can be found in the supabase UI. You can launch the test scripts and point to the above test database by running

./test.sh
