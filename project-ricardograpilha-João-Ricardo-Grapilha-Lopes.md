# MongoDb - Projeto Final
**Autor:** João Ricardo Grapilha Lopes
**Data:** 13/02/2016

**Versao:** 0.0.1

var usuario ={
	name:"", 
	bio:"",
	date_register:new Date(), 
	avatar_path:"",
	auth:[{
		username:"",
		email:"",
		password:"",
		last_access:new Date(),
		online:false,
		disabled:false,
		has_token:""
	}],
	settings_system:[{
		backgrong_path:""
	}]
};
//tipo do usuario
var type={
	name:""
}
var project={
	name:"",
	description:"",
	date_begin:new Date(), 
	date_dream:new Date(), 
	date_end:new Date(), 
	visible:"",
	realocate:"",
	expired:false,
	visualizable_mod:"",
	tags:[
		""
	],
	members:[{
		id_user:"",
		id_type:""
	}],
};
var notify_project={
	id_project: ObjectId,
	description:"",
	membres:[

	]
}
var goal={
	id_project: ObjectId,
	name:"",
	description:"",
	date_begin:new Date(), 
	date_dream:new Date(), 
	date_end:new Date(), 
	realocate:"",
	expired:false,
	date_realocate:new Date(),
	tags:[
		""
	]
};
var activity={
	id_goal: ObjectId,
	id_project: ObjectId,
	name:"",
	description:"",
	date_begin:new Date(), 
	date_dream:new Date(), 
	date_end:new Date(), 
	realocate:"",
	expired:false,
	date_realocate:new Date(),
	tags:[
		""
	]
};
var comment={
	text:"",
	date_comment:new Date(),
	files:[{
		path:"",
		weight:"",
		name:""
	}]
};
var notify_comment={
	id_project: ObjectId,
	id_comment: ObjectId,
	description:"",
	membres:[

	]
}
