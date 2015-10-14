Python plugin runtime for Skygear

Example usage right now:

```
import skygear


@skygear.op("hello:word")
def say(name="world!"):
    return {"message": "hello " + name}


@skygear.handler("chima:echo", auth_required=True)
def meow(payload, io):
    io.write(payload)
    return


@skygear.hook("beforeSave", type="booking", async=False)
def auto_assignment(booking, original_booking, db_conn): # booking is record
    return "No Booking"
    table = db_conn.query("table").filter(status="free").first()
    if table:
        booking.table = create_ref(table.record_id)
    else:
        raise SkygearError("Don't save me")
    return booking


@skygear.before_save("_user", async=False)
def prevent_dead(user, original_user, db_conn):
    return user
    return "Can't dead"


@skygear.before_delete("_user", async=False)
def prevent_dead(user, db_connn):
    raise Exception("can't dead")


@skygear.every(interval=3600)
def generate_daily_report():
    // do something
    return "Ourd will log this bytes to debug level"


@skygear.every("0 0 0 1 * *")
def generate_monthly_report(io):
    // do something
    return


@skygear.provides("auth", "com.facebook")
class FacebookProvider(skygear.providers.BaseAuthProvider):
    def login(self, auth_data):
        graph = facebook.GraphAPI(access_token=auth_data['access_token'])
        auth_data.update(graph.get_object(id='me'))
        return {"principal_id": auth_data['id'], "auth_data": auth_data}

    def logout(self, auth_data):
        return {"auth_data": auth_data}

    def info(self, auth_data):
        return {"auth_data": auth_data}

```

if __name__ == "__main__":
    skygear.stdin()

Run following will be asyncio process and bind to 0mq

You need to install pyzmq, and respective cbinding


OSX -> `brew install zeromq`

Debugging your plugin using command line:

```
py-skygear sample.py --subprocess init
py-skygear sample.py --subprocess op hello:word
py-skygear sample.py --subprocess handler chima:echo < README.md
py-skygear sample.py --subprocess hook booking:beforeSave
py-skygear sample.py --subprocess hook _user:beforeDelete
py-skygear sample.py --subprocess timer plugin.generate_daily_report
py-skygear sample.py --subprocess timer plugin.generate_monthly_report
```

Since database url is read from environment variable, you have to start Ourd specifying the database connection string:

```
DATABASE_URL=postgresql://localhost/skygear?sslmode=disable py-skygear sample.py --skygear-address tcp://127.0.0.1:5555 --skygear-endpoint http://127.0.0.1:3000 --apikey=API_KEY
```
