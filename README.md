# Laravel Lazy Loader on scrolling with Jquery

### In Conroller Method
```
 public function index( Request $request)
    {
        $users=User::paginate(15);
        if($request->ajax()){
            return $users;
        }
        return view('user',compact('users'));
    }
    
```
### In blade file

```
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Bootstrap demo</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0-beta1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-0evHe/X+R7YkIZDRvuzKMRqM+OrBnVFBL6DOitfPri4tjfHxaWutUpFmBp4vmVor" crossorigin="anonymous">
  </head>
  <body>
    <div class="container">
        <div class="row">
            <div class="col-12">
              <h1>User Information</h1>
            </div>
        </div>
        <div class="row" id="userInformation">
            @foreach ($users as $user)
          <div class="col-12">
              <div class="card">
                  <div class="card-body">
                      <h2>{{$user->id}} |{{$user->name}}</h2>
                      <p>{{$user->email}}</p>
                  </div>
              </div>
          </div>
            @endforeach
        </div>
        <div class="loading_img d-block text-center" id="loader_section" style="display: none">
            <img src="https://c.tenor.com/Tu0MCmJ4TJUAAAAC/load-loading.gif" alt="">
        </div>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0-beta1/dist/js/bootstrap.bundle.min.js"></script>
    <script src="https://code.jquery.com/jquery-2.2.4.min.js" integrity="sha256-BbhdlvQf/xTY9gja0Dq3HiwQF8LaCRTXxZKRutelT44=" crossorigin="anonymous"></script>

    <script>

        function loadData(pageID){
            $.ajax({
                url:'?page='+pageID,
                type:'get',
                beforeSend:function(){
                    $('#loader_section').show();
                }

            })
            .done(function(response){
                var userInfo='';
                if(response.data <1){
                    $('#loader_section').html('<h2 class="text-center text-danger">No More Data Found...</h2>');
                    return;
                }
                $.each(response.data,function(index,value){
                    userInfo+=`
                    <div class="col-12">
                        <div class="card">
                            <div class="card-body">
                                <h2>${value.id} | ${value.name}</h2>
                                <p>${value.email}</p>
                            </div>
                        </div>
                    </div>
                    `;
                });
                $('#userInformation').append(userInfo);
                $('#loader_section').hide();
            })
            .fail(function(){
                alert('Something went wrong...');
            })
        }

        var pageID =1;

        $(window).scroll(function(){

            if($(window).scrollTop() + $(window).height() >= $(document).height()){
                pageID++;
                loadData(pageID);
            }
        })
    </script>
  </body>
</html>
```
