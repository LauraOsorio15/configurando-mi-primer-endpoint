# configurando-mi-primer-endpoint
const axios = require('axios');

app.post('/dar-like', async (req, res) => {
  const { id_foto_instagram } = req.body;

  // Realizar la solicitud a la API de Instagram para dar like
  const response = await axios.post(`https://api.instagram.com/v1/media/${id_foto_instagram}/likes`, {
    access_token: 'TU_ACCESS_TOKEN', // Reemplazar con tu token de acceso de Instagram
  });

  // Verificar la respuesta y enviar la respuesta correspondiente al cliente
  if (response.data.meta.code === 200) {
    res.status(200).json({ mensaje: 'Like exitoso' });
  } else {
    res.status(response.data.meta.code).json({ error: response.data.meta.error_message });
  }
});

app.post('/dar-like', async (req, res) => {
  const { id_foto_instagram, id_usuario_instagram, id_dispositivo } = req.body;

  // Realizar la solicitud a la API de Instagram para dar like
  // ...

  // Registrar el like en tu base de datos
  const databaseRef = admin.database().ref('/likes');
  databaseRef.push({
    id_foto_instagram,
    id_usuario_instagram,
    id_dispositivo,
    timestamp: Date.now(),
  });

  // Enviar notificación push (ver siguiente paso)
  // ...

  res.status(200).json({ mensaje: 'Like exitoso' });
});

const messaging = admin.messaging();

// Enviar notificación push
function enviarNotificacion(token, mensaje) {
  const payload = {
    notification: {
      title: 'Nuevo Like',
      body: mensaje,
    },
  };

  messaging.sendToDevice(token, payload)
    .then(response => {
      console.log('Notificación enviada con éxito:', response);
    })
    .catch(error => {
      console.error('Error al enviar notificación:', error);
    });
}
