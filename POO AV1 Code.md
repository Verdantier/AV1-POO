using System;

public class Program
{
    public static void Main()
    {

        ICanal whatsAppProfessor = new WhatsApp();
        ICanal instaAluno = new Instagram();
        ICanal telegramCanal = new Telegram();

        MensagemBasica mensagem = new MensagemBasica();
        mensagem.DataEnvio = DateTime.Now;
        mensagem.Mensagem = "Oiiii, vamos assistir Sweet Home 2?";

        Video mensagemVideo = new Video();
        mensagemVideo.DataEnvio = DateTime.Now;
        mensagemVideo.Mensagem = "Segue o Trailer de Sweet Home 2!";
        mensagemVideo.Arquivo = "Sweet Home 2 - Official Trailer.MP4";
        mensagemVideo.Formato = FileType.MP4;
        mensagemVideo.Duracao = 60;

        Foto mensagemFoto = new Foto();
        mensagemFoto.DataEnvio = DateTime.Now;
        mensagemFoto.Mensagem = "Olha uma foto do Hyun Soo transformado";
        mensagemFoto.Arquivo = "Cha Hyun Soo.JPG";
        mensagemFoto.Formato = FileType.JPG;
		
		Audio mensagemAudio = new Audio();
		mensagemAudio.DataEnvio = DateTime.Now;
		mensagemAudio.Mensagem = "Escuta esse Podcast antes de assistir a segunda temporada";
		mensagemAudio.Arquivo = "Podcast - Sweet Home.MP3";
		mensagemAudio.Formato = FileType.MP3;

        // Enviando mensagem básica
        whatsAppProfessor.EnviarMensagem("+5511964687373", mensagem);

        // Enviando mensagem de vídeo
        instaAluno.EnviarMensagem("@alvrinhotbz", mensagemVideo);

        // Enviando mensagem de foto
        telegramCanal.EnviarMensagem("+5511964687373", mensagemFoto);

        MensagemMultimidia mensagemCaio = mensagemAudio;
        mensagemCaio.Mensagem = "Caio, pelo amor de DEUS escuta esse Podcast antes de assistirmos a segunda temporada de Sweet Home!";

        ICanal facebook = new Facebook();
        facebook.EnviarMensagem("Caio Vasconcelos", mensagemCaio);

        ICanal canal = Factory.Create("facebook");
    }
}

public class Foto : MensagemMultimidia
{
}

// Restante do código permanece inalterado

public enum FileType
{
    MP4,
    PDF,
    JPG,
	MP3
}

public static class Factory
{

  public static ICanal Create(string canal)
  {	
    switch(canal)
    {
      case "whatsApp":
        return new WhatsApp();
      case "facebook":
        return new Facebook();
      default:
      return null;
      break;
    };
    return null;	
  }
}

public interface ICanal
{
  void EnviarMensagem(string destinatario, MensagemBasica mensagem);

  void EnviarMensagem(string destinatario, MensagemMultimidia mensagem);

  void EnviarMensagem(string destinatario, Video mensagem);
}

public abstract class CanaisMensagem : ICanal
{
  protected abstract string canal {get;}

  public void EnviarMensagem(string destinatario, MensagemBasica mensagem)
  {

    Console.WriteLine("Mensagem básica enviada pelo canal: " + canal);
    Console.WriteLine("Destinatário: "+ destinatario);
    Console.WriteLine("Mensagem: "+ mensagem.Mensagem);
    Console.WriteLine("Data Envio: "+ mensagem.DataEnvio);
  }

  public void EnviarMensagem(string destinatario, MensagemMultimidia mensagem)
  {
    Console.WriteLine("Mensagem multimidia enviada pelo canal: " + canal);
    Console.WriteLine("Destinatário: "+ destinatario);
    Console.WriteLine("Mensagem: "+ mensagem.Mensagem);
    Console.WriteLine("Data Envio: "+ mensagem.DataEnvio);
    Console.WriteLine("Arquivo: "+ mensagem.Arquivo);
    Console.WriteLine("Tipo Arquivo: "+ mensagem.Formato);		
  }

  public void EnviarMensagem(string destinatario, Video mensagem)
  {
    Console.WriteLine("Mensagem video enviada pelo canal: " + canal);
    Console.WriteLine("Destinatário: "+ destinatario);
    Console.WriteLine("Mensagem: "+ mensagem.Mensagem);
    Console.WriteLine("Data Envio: "+ mensagem.DataEnvio);
    Console.WriteLine("Arquivo: "+ mensagem.Arquivo);
    Console.WriteLine("Tipo Arquivo: "+ mensagem.Formato);	
    Console.WriteLine("Duração: "+ mensagem.Duracao);	
  }
}

public class WhatsApp : CanaisMensagem, ICanal
{
  protected override string canal { get {return "WhatsApp";}}
}

public class Telegram : CanaisMensagem, ICanal
{
  protected override string canal { get {return "Telegram";}}
}

public class Instagram : CanaisMensagem, ICanal
{
  protected override string canal { get {return "Instagram";}}
}

public class Facebook : CanaisMensagem, ICanal
{
    protected override string canal { get {return "Facebook";}}
}


public class MensagemBasica
{
  public string Mensagem {get; set;}
  public DateTime DataEnvio {get; set;}
}

public class MensagemMultimidia : MensagemBasica
{
  public string Arquivo {get; set;}
  public FileType Formato {get; set;}
}

public class Video : MensagemMultimidia
{
  public int Duracao {get; set;}
}
public class Audio : MensagemMultimidia
{
  public int Duracao {get; set;}
}
