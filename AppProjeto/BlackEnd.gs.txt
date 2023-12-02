//====Variavel Grobal=======
//Planilha banco de dados...
var sheetsDb = SpreadsheetApp.openByUrl("caminho/url da plnilha google"); 
//Abas do banco dados.......................................................................................
var historico = sheetsDb.getSheetByName('historico');
//////////////////////////////////////////////////////////////////////////////////////////////////////////////
//----------Conexão WEb Form--------------------------------
function doGet(e){
  var Page = e.parameter.Page
  //return HtmlService.createHtmlOutputFromFile(Page || 'Index').addMetaTag('viewport', 'width=device-width, initial-scale=1').setTitle('Controle MERCADO')
   //Incluir CDN no Index.html

  //return render("Index");
  return HtmlService.createTemplateFromFile('Index').evaluate();
}

function include(filename){
  return HtmlService.createHtmlOutputFromFile(filename).getContent();
}
/////////////////////////////////////////////////////////////////////////

//---------Conexão formulario Index------------------------
function userCliecked(userInfo){
  //Criardo Codigo ID
  var codID = Number(historico.getLastRow());
   
    if (userInfo.sIDcheio != "" & userInfo.sIDcheio != null){
      var range = historico.getRange("A1:A").createTextFinder(userInfo.sIDcheio).findAll();

      for(var i=0; i<range.length; i++){
        historico.getRange(range[i].getRow(),2).setValue(userInfo.xProd);
        historico.getRange(range[i].getRow(),3).setValue(userInfo.xVal);
        historico.getRange(range[i].getRow(),4).setValue(userInfo.xQuant);
        historico.getRange(range[i].getRow(),5).setValue(userInfo.xSubT);
        historico.getRange(range[i].getRow(),6).setValue(userInfo.xTotal);
        historico.getRange(range[i].getRow(),7).setValue(new Date());
        //Mata FOR e encontra somente 1 registro
        if(historico.getRange(range[i].getRow(),2).getValue() != ""){
            i = (range.length * "10000");
        }
      }
    };
    historico.appendRow([codID,userInfo.xProd,userInfo.xVal,userInfo.xQuant,userInfo.xSubT,userInfo.xTotal,new Date()]);     
}
/////////////////////////////////////////////////////////////////////

