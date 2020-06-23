# twilio_laravel
Integration of laravel with twilio in php

Step by Step instructions for making programmable voice calls. visit https://divyaarajeshbabu.wordpress.com/integration-of-php-laravel-with-twilio/

create VIEW file in your project 
welcome.blade.php 
 <form action="{{route('initiate_call')}}" method="POST">
                @csrf
                <div class="form-group row">
                    <label for="phoneNumber" class="col-sm-2 col-form-label">Phone Number</label>
                    <div class="col-sm-10">
                        <input type="tel" name="phoneNumber[]" class="form-control" id="phoneNumber[]"
                            placeholder="Phone number">
                    </div>
                </div>
                <div class="form-group row">
                    <label for="phoneNumber1" class="col-sm-2 col-form-label">Phone Number1</label>
                    <div class="col-sm-10">
                        <input type="tel" name="phoneNumber[]" class="form-control" id="phoneNumber[]"
                            placeholder="Phone number1">
                    </div>
                </div>
                 <div class="form-group row">
                    <label for="text" class="col-sm-2 col-form-label">Message</label>
                    <div class="col-sm-10">
                        <input type="text" name="text" class="form-control" id="text"
                            placeholder="message">
                    </div>
                </div>
                <button type="submit" class="btn btn-primary">Call Me</button>
            </form>
create controller file VoiceController.php

public function initiate_call() {
   
    	$this->validate($request, [
    			'phoneNumber' => 'required|array',
    			]);
  
    	 
      
     $phonenumber=$request['phoneNumber'];
     $message=$request['text'];
     //echo $message;exit;

     foreach( $phonenumber as $to_number ) {

        $account_sid = getenv("TWILIO_SID");
        $auth_token = getenv("TWILIO_AUTH_TOKEN");
        $twilio_number = getenv("TWILIO_NUMBER");

        $client = new Client($account_sid, $auth_token);
       // $to_number = $client->lookups->v1->phoneNumbers($to_number)->fetch();
  

        $client->account->calls->create(
            $to_number,
            $twilio_number,
            [

        "twiml" => "<Response>

<Gather action='http://52.186.153.10/test.php' numDigits='1' method='GET'>
<Say>If you get your order press 1</Say>
    </Gather><Say>Thank You !!</Say>
</Response>"
            ]
        );
     }
        
     return  'You will receive a call shortly!';
        //return true;
    
  }

Make routes for views and controller with the file web.php
Route::view('/', 'welcome');
Route::post('/call', 'VoiceController@initiateCall')->name('initiate_call');
